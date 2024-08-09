## Open Event Viewer:
- Launch Event Viewer from the Start menu or by typing eventvwr in the Run dialog.
## Identify Total Logs for EventID 11:
- Method 1: Filter the logs displayed in Event Viewer for EventID 11 and manually count the entries or check the displayed total number.
- Method 2: Use a script or tool to automate the counting process for efficiency.
## Analyze Sysmon Logs:
- Utilize the Ultimate Windows Security Encyclopedia to understand the meaning of each EventID.
## Identify Malicious Processes:
- Step 1: Filter the logs to display entries with EventID 1.
- Step 2: Review the six results obtained. Look for logs with suspicious binary names, such as those with double extensions.
- Step 3: Verify the suspicious binaries by sending their hash values to VirusTotal for analysis.
## Confirm Malware Presence:
- If VirusTotal tags the file as malicious (e.g., categorized as a Trojan from the winvnc family), proceed with further investigation.
## Trace Malware Activity:
- Step 1: Identify access to cloud storage services (e.g., Dropbox) from the victim’s system using EventID 22 (DNS Event).
- Step 2: Analyze subsequent EventID 11 logs to trace file creation events. Look for partially downloaded files (e.g., .part files) and completed downloads.
## Verify File Modifications:
- Step 1: Filter logs for EventID 2 to identify attempts to modify file creation times.
- Step 2: Review the logs to find entries indicating timestamp modifications, such as changes to PDF file timestamps.
## Investigate Dropped Files:
- Step 1: Identify additional files dropped by the malicious process (e.g., once.cmd).
- Step 2: Check the full path of these files in the log’s general tab.
## Identify Dummy Domains:
- Step 1: Filter logs for EventID 22 to find dummy domains accessed by the malicious file.
- Step 2: Review the logs to identify domains such as www.example.com.
## Monitor Network Connections:
- Step 1: Filter logs for EventID 3 to identify network connection attempts by the malicious process.
- Step 2: Review the logs to find connection attempts to specific IP addresses (e.g., 93.184.216.34).
## Track Process Termination:
- Step 1: Filter logs for EventID 5 to identify process termination events.
- Step 2: Review the logs to determine the timestamp of the termination.
