## Access the SQLite3 Database Dump:
- Start by accessing the SQLite3 database dump. Run the query:
`SELECT name FROM sqlite_master WHERE type='table';`
- This will list the tables, and one of them should catch your attention.
- Identify the Username of the External Contractor:
## Execute the following query to view all columns and their values:
`SELECT * FROM phpbb_users;`
- You’ll notice a username associated with a contractor’s email, indicating that the external contractor is likely “apoole” or “apoole1”.
## Identify the IP Address:
- Check the relevant column value to identify the IP address associated with the contractor.
## Find the Malicious Post:
- Enumerate the tables in the database and look for the post_id column. The phpbb_posts table is of interest.
- At post_id 9, the poster_ip matches the contractor’s IP address. Analyzing the post_text shows that the contractor’s post is indeed malicious, indicating an attempted XSS attack to steal users’ cookies.
## Determine the URI Path:
- Analyze the malicious content. Searching for the attacker’s IP address should reveal the URI path.
## Find the UTC Time:
- First, analyze the access.log file but if no supporting evidence is found, examine the phpbb_log table.
- Look for a column named log_operation, which indicates a successful admin login attempt.
## Retrieve the Password Name:
- Use the strings command on the database and grep for “ldap” to retrieve the password name.
## Check the User Agent:
- Check the user agent for the IP address 10.255.254.2, as this IP was previously identified as belonging to Forela.
## Check the Log Operation for Time:
- Again, check the log_operation to get the time. After the attacker logged in as admin, they added their own user account to the admin group.
- To find the timestamp, convert the epoch time to UTC.
## Find Evidence in the access.log File:
- In the phpbb_log table, note that the attacker attempted a database backup, with the timestamp added to the backup’s filename. This confirms the backup file.
- From the log entry, a GET request shows the attacker tried to download the backup file. To get the UTC timestamp, subtract 1 from the log’s timestamp, which is +1.
## Check Status Code and Request Length:
- Besides the status code, check the bytes or lengths for the current request.
