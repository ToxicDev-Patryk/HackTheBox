## Question 1
In this task, we are given a Unix auth.log and wtmp logs. Upon reviewing the log traffic, we can identify two IP addresses: 203.101.190.9 and 65.2.161.68. The first IP attempted to log in as root and was authenticated at 06:19:54. This single attempt could indicate a normal user or the rightful owner of the account. The second IP made several attempts to log in as admin starting at 06:31:31. Checking the IP’s reputation reveals it is marked as suspicious but not as an abuse IP. However, the massive failed login attempts from this IP raise security concerns, marking it as the suspect.

## Question 2
Further analysis shows the initial attempt from the suspect IP to log in as root. After several brute force attempts, the suspect successfully authenticated as root at 06:31:40 but logged out shortly thereafter.

## Question 3
Identifying the initial manual entry is tricky. Further analysis reveals that the attacker manually logged in as root at 06:32:44. However, 06:32:44 is the timestamp for the successful login attempt. Therefore, the timestamp for the account’s access should be 06:32:45.

## Question 4
After the attacker manually logged in to the root account, a new session number was created: 37.

## Question 5
For lateral movement, the attacker created a new user and group named “cyberjunkie”. The attacker then added “cyberjunkie” to the sudo group, allowing it to execute sudo permissions or access any files requiring sudo.

## Question 6
Researching MITRE ATT&CK, we find that persistence has several techniques and sub-techniques. Based on the attacker’s actions, the correct technique is “Create Account” with “Local Account” as the sub-technique.

## Question 7
Reviewing the wtmp log file, we confirm that the attacker’s session for the root account was terminated at 06:37 (UTC+8). To identify the session duration, review the auth.log for session removal or closure entries. The session duration is calculated as: 06:32:45 - 06:37:24 --> 279 seconds.

## Question 8
Additionally, the attacker logged in to the server as “cyberjunkie” and executed a sudo command at 06:39:38.
