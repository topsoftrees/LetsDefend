# SOC141 - Phishing URL Detected - Case 86 
We received a high alert for the following information:

>**EventID:** 86
>
>**Event Time:** March 22, 2021, 9:23 p.m.
>
>**Rule:** SOC141 - Phishing URL Detected
>
>**Level:** Security Analyst
>
>**Source Address:** 172.16.17.49
>
>**Source Hostname:** EmilyComp
>
>**Destination Address:** 91.189.114.8
>
>**Destination Hostname:** mogagrocol.ru
>
>**Username:** ellie
>
>**Request URL:** http://redacting-the-malicious-site.com
>
>**User Agent:** Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36
>
>**Device Action:** Allowed

## Analysis
Going to Log Management, we do a search for the source IP address - 172.16.17.49

<img width="627" alt="Screen Shot 2022-06-23 at 9 38 21 AM" src="https://user-images.githubusercontent.com/74877876/176001196-b0914367-f91c-45e5-adc8-49aa116330b6.png">

Looking at the last two entries (the destination address), the requested URL was http://redacting-the-malicious-site.com on March 22, 2021 at 9:23 p.m. (which is time of the detected alert).

Doing a search of the requested URL in VirusTotal, it’s been reported as phishing:<img width="884" alt="Screen Shot 2022-06-23 at 9 40 44 AM" src="https://user-images.githubusercontent.com/74877876/176001964-60f00f9a-c004-4555-b78c-a2093fdc67eb.png">

We see VirusTotal doesn’t pick up on an IP address. 


Using another threat intelligence platform, urlscan.io detects the web address as malicious and a serving DNS record of 91.189.114.8. This is the destination address. This appears to be a phishing attempt in which the user gave their credentials.

## Determination
True positive

Confirmed credential phishing site was active at the time of alert. Assuming user is connected to a domain, disable the AD account until passwords are changed. 

## Other Detections
In the command history for 172.16.17.49, all events take place before the event was detected. The last event was February 14, 2021 at 12:12 p.m.

In the browser history, the last events were December 5, 2020. 

In the process history, processes such as Adobe Acrobat, Chrome, Notepad, and an HTML application hashes have either been verified with VirusTotal from Microsoft and I verified the process ID. One process that was detected multiple times as malware (according to VirusTotal), most likely a Trojan, KBDYAK.exe with MD5 hash as a4513379dad5233afa402cc56a8b9222. There aren’t any signatures from the files. 

In network connections, on February 14, 2021, there was a connection to an IP address that’s known to be associated with phishing. Following that event, the next minute, the device connected to an IP address that’s known to be malware which is the executable file listed above.

Confirmed malware active on February 14, 2021. 

## Helpful Websites
- [AnyRun](https://app.any.run/)
- [VirusTotal](https://www.virustotal.com/)
- [URLHouse](https://urlhaus.abuse.ch/browse/)
- [URLScan](https://urlscan.io/)
- [HybridAnalysis](https://www.hybrid-analysis.com/)


