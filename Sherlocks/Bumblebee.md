## Question 1
We are given two files: a log file and an SQLite3 database file. To find the username of the external contractor, start by accessing the SQLite3 database dump. Run the query: SELECT name FROM sqlite_master WHERE type='table';. This will list the tables, and one of them should catch your attention. Next, execute: SELECT * FROM phpbb_users; to view all columns and their values. You’ll notice a username associated with a contractor’s email, indicating that the external contractor is likely “apoole” or “apoole1”.

## Question 2
To identify the IP address, simply check the relevant column value.

## Question 3
To find the malicious post made by the contractor, enumerate the tables in the database and look for the post_id column. The phpbb_posts table is of interest. At post_id 9, the poster_ip matches the contractor’s IP address. Analyzing the post_text shows that the contractor’s post is indeed malicious. It appears the contractor attempted an XSS attack to steal users’ cookies.

## Question 4
To determine the URI path, analyze the malicious content. Searching for the attacker’s IP address should reveal the URI path.

## Question 5
To find the UTC time, I first analyzed the access.log file but found no supporting evidence. Then, I examined the phpbb_log table and found a column named log_operation, which indicates a successful admin login attempt.

## Question 6
To retrieve the password name, use the strings command on the database and grep for “ldap”.

## Question 7
Check the user agent for the IP address 10.255.254.2, as we previously identified this IP as belonging to Forela.

## Question 8
Again, check the log_operation to get the time. After the attacker logged in as admin, they added their own user account to the admin group. To find the timestamp, convert the epoch time to UTC.

## Question 9
I found evidence in the access.log file. In the phpbb_log table, it was noted that the attacker attempted a database backup, with the timestamp added to the backup’s filename. This confirms the backup file. From the log entry, a GET request shows the attacker tried to download the backup file. To get the UTC timestamp, subtract 1 from the log’s timestamp, which is +1.

## Question 10
Besides the status code, check the bytes or lengths for the current request.

