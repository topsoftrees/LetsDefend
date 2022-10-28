# SOC137 - Malicious File/Script Download Attempt

We received a Medium alert for the possible malware:

> **EventID:** 76
> 
> **Event Time:** March 14, 2021, 7:15 p.m.
> 
> **Rule:** SOC137 - Malicious File/Script Download Attempt
> 
> **Level:** Security Analyst
> 
> **Source Address:** 172.16.17.37
> 
> **Source Hostname:** NicolasPRD
> 
> **File Name:** INVOICE PACKAGE LINK TO DOWNLOAD.docm
> 
> **File Hash:** f2d0c66b801244c059f636d08a474079
> 
> **File Size:** 16.66 Kb
> 
> **Device Action:** Blocked
> 
> **Download (Password:infected):** f2d0c66b801244c059f636d08a474079.zip


## Analysis

VirusTotal: Looking at the hash, INVOICE PACKAGE LINK TO DOWNLOAD.docm matches the hash. 104.21.13.139, 172.67.200.96, and 178.175.67.109 but it doesn't look like there are any connections in the logs. 


## Determination

## Helpful Links
