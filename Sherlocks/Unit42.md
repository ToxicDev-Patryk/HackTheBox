Step 1: Identify the Correct Profile
To begin RAM forensics, we need to identify the correct profile. We can use Volatility’s imageinfo plugin for this purpose. The first suggested profile is usually the correct one. From the results, we can determine that the operating system is Windows 7.

Step 2: Determine Memory Dump Creation Time
The imageinfo plugin also provides information about the time when the memory dump was created, specified in the Image Date and Time header.

Step 3: Identify Data on the Clipboard
To identify data saved on the clipboard, use the clipboard plugin in Volatility 2. If the first data shown appears obfuscated, it could indicate the obfuscation mentioned in the questions. To verify if this is the copied command, use the cmdscan plugin to check the commands executed by the powershell.exe process. The results should show that the command was executed in both cmd and PowerShell, confirming that the copied command is of interest.

Step 4: Identify the Targeted PowerShell Cmdlet
To identify the targeted PowerShell cmdlet, use the consoles plugin to check the result of the executed obfuscated command. The result should show iex, which stands for Invoke Expressions. Researching iex reveals that it is used to set an alias for the Invoke Expression cmdlet.

Step 5: Analyze CMD History
By analyzing the CMD history, we can identify an attempt by the attacker to exfiltrate the content of Confidential.txt to a file named pass.txt inside the pulice directory. However, the exfiltration failed because the program path was not found.

Step 6: Analyze Terminal History
Further analysis of the terminal history reveals encoded messages. After decoding the messages, we can identify the full path of the readme file.

Step 7: Check Hostname
To check the hostname in Windows, run the command net users. From the terminal history, the hostname of the compromised system is USER-PC. Alternatively, we can dump the registry key to find the hostname. First, find the offset of \REGISTRY\MACHINE\SYSTEM using the hivelist plugin. Then, use the printkey plugin to dump the registry key.

Step 8: Identify Total User Accounts
To identify the total user accounts on the machine, use the hashdump plugin. The results should show that there are three registered users. To identify the full location path, use the filescan plugin and filter for passwords.txt.

Step 9: Identify Malicious Executable File
By analyzing the results of the console plugin, we can identify an executable file with a hash-like name. To verify if it is malicious, use VirusTotal to check the file. The filename, being the hash value of itself, can be used to confirm its malicious nature. Open the details tab in VirusTotal to check the imphash value and the creation time of the executable file.

Step 10: Identify Local IP Address
To identify the local IP address of the machine, use the netscan plugin. The results should show that 192.168.0.104 is the local IP address, as it listens to 0.0.0.0.

Step 11: Identify Parent Process of PowerShell
To identify the parent process of the powershell.exe process, use the pstree plugin to list all processes and their child processes. The results should show that cmd.exe is the parent process.

Step 12: Identify Browser Used for Social Media Login
The attacker likely used a browser to log in to social media. Dump the memory of the browser executable file. From the previous pslist results, we identified that Microsoft Edge was used. In Volatility, use the memmap plugin to dump the memory. Then, use the strings command on the .dmp file and search for .gmail.com. The results should reveal a .gmail.com account and URL-encoded data. Decoding the URL shows that mafia_code1337@gmail.com was used for login.

Step 13: Identify the SIEM Solution Name
To identify the SIEM solution name, use the filescan plugin and search for “history” with the -i flag for case-insensitive search. The relevant data is found at offset 0x11e0d16f0, which stores all the browser history. Dump this data using the dumpfiles plugin. The .dat file contains the history. Open it in Linux using the open command, then navigate to the keyword_search_terms table and browse the data. The results indicate that the victim searched for “WAZUH,” which is the name of the SIEM solution.

Step 14: Identify the Malware Executable File
To identify the malware executable file, list all files in the /Downloads directory. Notice a process named csrsss.exe, which is a typo (the “s” character should not appear more than twice). This indicates that the malware executable file downloaded by the user is csrsss.exe.

Step 15: Analyze Event Logs for Malicious Activity
To analyze this type of log file, we can use Event Viewer. There are two ways to identify the total logs for EventID 11: filter the logs displayed in Event Viewer and count them manually or check the top displayed number. In analyzing Sysmon logs, I used an online wiki to help identify the meaning of each EventID. It indicated that a malicious process infected the victim’s system, suggesting the process was created in memory and should be logged. To hunt the malicious process, filter the logs displayed to those with EventID 1. After reviewing the logs, one log with a suspicious binary name (double extension) was found. Sending the hash values provided in the Sysmon log to VirusTotal confirmed it as malicious, categorized as a Trojan from the winvnc family.

Step 16: Identify Dropbox Access and Malware Download
The first Sysmon log identified access to Dropbox from the victim’s system (EventID 22 indicates a DNSEvent). The next Sysmon log (EventID 11) indicates a file creation event. Reviewing the logs, we can speculate that Dropbox was accessed, and the malware was downloaded to the victim’s system. This is confirmed by reviewing subsequent EventID 11 logs, showing a .part file for skZdsnwf.exe, indicating an attempted but unfinished download. The final EventID 11 log shows firefox.exe used at the same timestamp, confirming the malware download.

Step 17: Identify File Creation Time Modification
To identify an attempt to modify file creation time, filter for EventID 2. The log shows a PDF file with a modified timestamp to make it appear old. The Details tab confirms the creation timestamp change. Further analysis shows the malicious file dropped several files, including once.cmd, with the full path shown in the log’s general tab.

Step 18: Identify Dummy Domain Accessed by Malicious File
To identify the dummy domain accessed by the malicious file, filter the logs shown with EventID 22 (DNSEvent). Reviewing the logs, we can identify that www.example.com is the dummy domain accessed by the malicious file.

Step 19: Identify Network Connection Attempt
If a process tries to reach a network connection, a Sysmon log with EventID 3 is created. Reviewing the content, we can identify that the malicious process tried to establish a connection to 93.184.216.34.

Step 20: Identify Process Termination
At a certain time, the malicious file terminated its process after infecting the PC with a backdoored variant of UltraVNC. If a process is terminated, EventID 5 is created. Reviewing the log’s content, we can identify the timestamp of the termination.
