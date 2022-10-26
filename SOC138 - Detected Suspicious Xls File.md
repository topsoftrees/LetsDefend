# SOC138 - Detected Suspicious Xls File

We received a Medium alert for the following: 

> **EventID:** 77
> 
> **Event Time:** March 13, 2021, 8:20 p.m.
> 
> **Rule:** SOC138 - Detected Suspicious Xls File
> 
> **Level:** Security Analyst
> 
> **Source Address:** 172.16.17.56
> 
> **Source Hostname:** Sofia
> 
> **File Name:** ORDER SHEET & SPEC.xlsm
> 
> **File Hash:** 7ccf88c0bbe3b29bf19d877c4596a8d4
> 
> **File Size:** 2.66 Mb
> 
> **Device Action:** Allowed
> 
> **Download (Password:infected):** 7ccf88c0bbe3b29bf19d877c4596a8d4.zip

## Analysis
We have the following connections from 172.16.17.56 at the time of the event. The log details show what seem to be random character data with the final piece being multiwaretecnologia.com.br. 

![Screen Shot 2022-10-25 at 3 36 40 PM](https://user-images.githubusercontent.com/74877876/197865955-b24c4eaa-fa47-460c-af75-d8e5c2368398.png)

No Endpoint logs related to the time of the event. No emails too or from Sofia found. 

**VirusTotal:** Investigating the file hash, the hash is associated with file name ORDER SHEET & SPEC.xlsm and is known as a Trojan.
It's obvious the file is malcious but I don't see how it got on the machine. multiwaretecnologia.com.br is related to 
177.53.143.89. 

**Hybrid Analysis:** Another name for ORDER SHEET & SPEC.xlsm is 7ccf88c0bbe3b29bf19d877c4596a8d4.zip and shown to be malicious by a 100/100 threat score. Can't find information on the weird data. 

**AnyRun:** Looked at ORDER SHEET & SPEC.xlsm which shows DNS requests responding to multiwaretecnologia.com.br, 177.53.143.89 - the destination address. Looks like the Excel spreadsheet tries to load then and command is executed. 
![Screen Shot 2022-10-26 at 9 37 24 AM](https://user-images.githubusercontent.com/74877876/198040674-2c8fd703-8804-4a18-8485-7e9d23a2ec28.png)

Cmd executes C:\Users\admin\AppData\Local\Temp\v with has ef556c44786a88cdf0f705ac03d9099a. VirusTotal sees this hash is associated with ORDER SHEET & SPEC.xlsm.
![Screen Shot 2022-10-26 at 9 47 24 AM](https://user-images.githubusercontent.com/74877876/198043295-16f3da77-3899-41b6-b49e-1568a905202d.png)

The command runs wscript.exe and the hash is 03d7df9993352270e6a5497b895e79a8. That hash is associated with ORDER SHEET & SPEC.xlsm in VirusTotal. 
![Screen Shot 2022-10-26 at 9 37 00 AM](https://user-images.githubusercontent.com/74877876/198040577-77f01a56-63e5-469f-bb5d-5c14742b3578.png)

True Positive, confirmed suspicious xls file from malicious domain. Destination address belongs to multiwaretecnologia.com.br which pulled the malicious xls file. Executed commands hashes are associated with malicious xls file.

## Determination
True Positive, confirmed suspicious xls file from malicious domain. Destination address belongs to multiwaretecnologia.com.br which pulled the malicious xls file. Executed commands hashes are associated with malicious xls file.

## Helpful Links
- VirusTotal: https://www.virustotal.com/gui
- HybridAnalysis: https://hybrid-analysis.com/
- AnyRun: https://app.any.run/tasks

