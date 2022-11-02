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

**VirusTotal:** Looking at the hash, INVOICE PACKAGE LINK TO DOWNLOAD.docm matches the hash. 104.21.13.139, 172.67.200.96, and 178.175.67.109 but it doesn't look like there are any connections in the logs. 

![Screen Shot 2022-11-02 at 12 49 48 PM](https://user-images.githubusercontent.com/74877876/199551262-6e03ed83-1cb1-455c-a15e-20e6e0aca4c8.png)

We see in the logs, the following connections: 
![Screen Shot 2022-11-02 at 12 54 53 PM](https://user-images.githubusercontent.com/74877876/199552485-92626683-625f-455d-b64d-1f9d9408f42f.png)

- There's a connection to 49.51.12.195 requesting iluuryeqa.info which is known as spyware. Parent process is wmiprvse.exe.
- There's a connection to 31.214.157.60 requesting http://ueba6ka.club/images/DVeUkINudhi79z0c_2Bv/hcMjSPUhHNICgDZ2eJc/uPkHWXvBmVjikkuyor3cnx/gi3BCr71LrlRP/OOmOS8_2/Bl7A_2Fjz2BM4Pth4RZbBKn/1OVxs19bE9/6wtE2QLgVO1DP1TCF/SoIEOJUXIYbo/RtuJbNDFWW5/V.avi which is known as spyware. At this time, I don't see any connections to iluuryeqa.info. 
- There's a connection to 49.51.12.195 requesting http://ueba6ka.club/favicon.ico which is known as spyware and is related to 31.214.157.60 request.





## Determination

## Helpful Links
