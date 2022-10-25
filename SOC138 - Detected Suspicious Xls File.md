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
We have the following connections from 172.16.17.56 at the time of the event. The log details show what seem to be random character data. 

![Screen Shot 2022-10-25 at 3 36 40 PM](https://user-images.githubusercontent.com/74877876/197865955-b24c4eaa-fa47-460c-af75-d8e5c2368398.png)

No Endpoint logs related to the time of the event. No emails too or from Sofia found. 

**VirusTotal:** Investigating the file hash, the hash is associated with file name ORDER SHEET & SPEC.xlsm and is known as a Trojan.
It's obvious the file is malcious but I don't see how it got on the machine. 

**Hybrid Analysis:** Another name for ORDER SHEET & SPEC.xlsm is 7ccf88c0bbe3b29bf19d877c4596a8d4.zip and shown to be malicious by a 100/100 threat score. Can't find information on the weird data. 

**CyberChef:** Converted and did not find data related to the output. 

## Determination


## Helpful Links
- VirusTotal: 
- HybridAnalysis:
- CyberChef: https://gchq.github.io/CyberChef/










