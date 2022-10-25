# SOC170 - Passwd Found in Requested URL
We received a High alert for the possible LFI attack: 

> **EventID:** 120
> 
> **Event Time:** March 1, 2022, 10:10 a.m.
> 
> **Rule:** SOC170 - Passwd Found in Requested URL - Possible LFI Attack
> 
> **Level:** Security Analyst
> 
> **Hostname:** WebServer1006
> 
> **Destination IP Address:** 172.16.17.13
> 
> **Source IP Address:** 106.55.45.162
> 
> **HTTP Request Method:** GET
> 
> **Requested URL:** https://172.16.17.13/?file=../../../../etc/passwd
> 
> **User-Agent:** Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)
> 
> **Alert Trigger Reason:** URL Contains passwd
> 
> **Device Action:** Allowed

## Analysis
We can see 106.55.45.162 port 49028 connects to 172.16.17.13 port 443 at 03/02/22 10:10 AM with the following information:
![Screen Shot 2022-10-24 at 12 21 41 PM](https://user-images.githubusercontent.com/74877876/197576047-c7b89c95-185e-4fa2-bd18-4c23de59405c.png)

**/etc/passwd:** user information

**/etc/shadow: **encrypted user password

**HTTP 500:** Internal server error - looks like it failed to access the file

I'm not too familiar with the Windows file structure, but I'm pretty sure /etc/passwd and /etc/shadow only exist on Linux distrobutions. 

**VirusTotal:** Looking at the source address, the IP has been community reported as trying to carry out a bruteforce attack. 
**Cisco Talos:** The source address belongs to China. 
**IBM X-Force Exchange:** Many community reports of SSH brute force attack. 

No emails found relating this this source addressed. Unknown how this was accessed on this device. 

## Determination 
False Positive, unknown how https://172.16.17.13/?file=../../../../etc/passwd was triggered on this Endpoint. Either way, 106.55.45.162 was unsuccessful in retrieving user password per HTTP 500 code (server error).
System was not compromised. 

Reporting as True Postive and confirmed the endpoint wasn't compromised. Reached out to LetsDefend to see how this is a True Positive since the device wasn't compromised. Are we just saying it did access the site or analyzing if this is compromised? Received confirmation this is a true positive as there was detection of a passwd found in the URL not that it was actually compromised. 

## Helpful Links
- **VirusTotal:** https://www.virustotal.com/gui
- **Cisco Talos:** https://www.talosintelligence.com/
- **IBM EX-Force Exchange:** https://exchange.xforce.ibmcloud.com/

