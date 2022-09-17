# SOC146 - Phishing Mail Detected - Excel 4.0 Macros	93	

This alert was generated from a real phishing attack.

We received a High alert for the following Exchange attack:
> **EventID:** 93
> 
> **Event Time:** June 13, 2021, 2:13 p.m.
> 
> **Rule:** SOC146 - Phishing Mail Detected - Excel 4.0 Macros
> 
> **Level:** Security Analyst
> 
> **SMTP Address:** 24.213.228.54
> 
> **Source Address:** trenton@tritowncomputers.com
> 
> **Destination Address:** lars@letsdefend.io
> 
> **E-mail Subject:** RE: Meeting Notes
> 
> **Device Action:** Allowed


## Analysis 
After a quick LetsDefend search of the alert information above, it looks like the destination address is associated with LarsPRD, 172.16.17.57. 

Here is the email for RE: Meeting Notes:

<img width="779" alt="Screen Shot 2022-09-17 at 12 15 45 PM" src="https://user-images.githubusercontent.com/74877876/190866320-46fd2cba-349e-470d-a3b2-f1567edb67d9.png">

Downloading the file and do some analysis using tools:
- VirusTotal: File comes back as a detected trojan. The name of the file comes back as associated with malware.
- HybridAnalysis: Detects two malicious .dll files, iroto.dll and iroto1.dll, and a .xls file. Following the HybridAnalysis links with the files shows indictors to the following process:
<img width="1165" alt="Screen Shot 2022-09-17 at 2 21 26 PM" src="https://user-images.githubusercontent.com/74877876/190871223-719a6934-bc7d-49ae-9680-c80f382c6583.png">

Going back to LetsDefend LarsPRD Endpoint:
- Command History: it looks like the regsvr32.exe runs iroto.dll and iroto1.dll at 13.06.2021 14:20
- Browser History: at 13.06.2021 14:20, the browser accessed https://royalpalm.sparkblue.lk/vCNhYrq3Yg8/dot.html
13.06.2021 14:21: https://nws.visionconsulting.ro/N1G1KCXA/dot.html. VirusTotal includes a few vendors detecting https://royalpalm.sparkblue.lk/vCNhYrq3Yg8/dot.html and https://nws.visionconsulting.ro/N1G1KCXA/dot.html as malware. 
- Network Connections: at 13.06.2021 14:20, accesses 188.213.19.81 and 192.232.219.67 which are the IPs of the websites just above. 
- Processes List: regsvr32.exe -s iroto.dll which launches Excel (verifed using hash)

Verified Lars did access the URLs which processed Excel and no other device accesses 188.213.19.81 and 192.232.219.67.

<img width="1096" alt="Screen Shot 2022-09-17 at 2 41 08 PM" src="https://user-images.githubusercontent.com/74877876/190871904-472b9144-2f29-41d6-bae3-fa902cb43fce.png">

Analyzing the source address:
- MXToolbox: tritowncomputers.com has the address 108.179.232.57, not 24.213.228.54. 

<img width="1137" alt="Screen Shot 2022-09-17 at 12 52 24 PM" src="https://user-images.githubusercontent.com/74877876/190867848-28d3f488-9825-4ac6-94ea-ad445ade7bcd.png">

However, searching for tritowncomputers.com in VirusTotal shows there's a relation to 24.213.228.54. 

<img width="892" alt="Screen Shot 2022-09-17 at 2 46 12 PM" src="https://user-images.githubusercontent.com/74877876/190872057-09f1b43e-fb14-47d0-b3b4-56db00f6ee22.png">

## Determination
After analysis, I've concluded that lars@letsdefend.io received file from 108.179.232.57 (similarly 24.213.228.54) which contained, iroto.dll and iroto1.dll, malicious files that were executed in Excel at the time of this alert. 

True Positive

## Helpful Links
- VirusTotal: https://www.virustotal.com/gui/home/upload
- HybridAnalysis: https://www.hybrid-analysis.com/ 
- MXToolbox: https://mxtoolbox.com/
 
