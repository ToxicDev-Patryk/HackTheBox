## Convert Raw MFT File to CSV Format:
- Use either analyzeMFT or MFTECmd.exe to convert the raw MFT file to a CSV format. Both tools serve the same purpose.
- For this writeup, MFTECmd.exe was used to convert the raw MFT to a CSV file.
## Upload CSV File to Timeline Explorer:
- Upload the converted CSV file to Timeline Explorer for analysis.
- Identify the ZIP Filename Downloaded by Simon:
- Search for .zip files and correlate the timestamps to identify the ZIP file downloaded on February 13th.
- Found one ZIP file that looks convincing based on its timestamp and file path. Another ZIP file was found in the Downloads directory, which is interesting.
## Improve Visibility:
- Sort the parent path and customize the filter to only show the Downloads directory of user Simon.
- Based on the results, there are only two .zip files in Simon’s Downloads directory on February 13, 2024.
## Further Analysis:
- Found that Stage-20240213T093324Z-001.zip was unzipped and contained another ZIP file named invoices.zip.
- Inside invoices.zip, a .bat file was found, indicating a malicious file. The file size is only 286 bytes, suggesting it is stored directly in the MFT entry.
## Analyze invoice.bat Content:
- Upload the raw MFT file using MFT Explorer to analyze the content of invoice.bat.
- The analysis reveals a C2 IP and its port, which act as the web server to download a file.
## Alternative Content Review:
- Another way to review the content of invoice.bat is to upload the raw MFT file to a hex editor and calculate the offset.
## Identify Full HOST URL:
- Open Timeline Explorer again, filter the malicious ZIP file, and check the Zone ID Contents column to identify the full HOST URL.
## Identify Full Path:
- Filter for the malicious file (invoice.bat) and look at the bottom for the full path.
## Identify File Creation Timestamp:
- Check the Created0x30 column to identify the timestamp for the file creation on disk.
## Calculate Offset:
- As an alternative way to investigate the .bat content, calculate the offset by multiplying the Entry Number by 1024 and converting it to hex.
## Review C2 IP and Port:
- Previously identified the C2 IP and its port by reviewing the .bat file content.
