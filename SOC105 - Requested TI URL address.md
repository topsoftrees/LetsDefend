# SOC105 - Requested T.I. URL address
We received a High alert for the following Threat Intelligence attack:

> **EventID:** 75
> 
> **Event Time:** March 7, 2021, 5:47 p.m.
> 
> **Rule:** SOC105 - Requested T.I. URL address
> 
> **Level:** Security Analyst
> 
> **Source Address:** 10.15.15.12
> 
> **Source Hostname:** MarksPhone
> 
> **Destination Address:** 67.199.248.10
> 
> **Destination Hostname:** bit.ly
> 
> **Username:** Mark
> 
> **Request URL:** https://bit.ly/TAPSCAN
> 
> **User Agent:** Mozilla/5.0 (Windows NT 5.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36
> 
> **Device Action:** Allowed


## Analysis
Looking at the source address in Log Management, we see 10.15.15.12 connects to 67.199.248.10 at Mar, 07, 2021, 05:47 PM. The requested URL was https://bit.ly/TAPSCAN. 

There's no recorded logs or recent emails to Mark. However, the domain, bit.ly, is considered spam on March 7, 2021, 5:29 p.m. according to the Threat Intelligence module in LetsDefend.

**VirusTotal:** Searching for https://bit.ly/TAPSCAN shows serving IP address of 67.199.248.11. CMC Threat Intelligence shows this as malicious. 

**HybridAnalysis:** Searching for https://bit.ly/TAPSCAN shows Falcon reporting malicious and connecting to 67.199.248.10. All IOCs show connecting to spam sites in the bit.ly and Google Play domains. According to the screenshots, this redirects to a Scanner App. 

## Determination
True Positive that malicious content was accessed. 

This was marked as False Positive, but it's pending review if it's actually False Positive. 

## Helpful Links
- **VirusTotal:** https://virustotal.com/gui
- **HybridAnalysis:** https://hybrid-analysis.com/
