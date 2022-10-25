# SOC175 - PowerShell Found in Requested URL
We received a High alert for this possible CVE-2022-41082 exploitation.

> **EventID:** 125
> 
> **Event Time:** Sept. 30, 2022, 7:19 a.m.
> 
> **Rule:** SOC175 - PowerShell Found in Requested URL - Possible CVE-2022-41082 Exploitation
> 
> **Level:** Security Analyst
> 
> **Hostname:** Exchange Server 2
> 
> **Destination IP Address:** 172.16.20.8
> 
> **Log Source:** IIS
> 
> **Source IP Address:** 58.237.200.6
> 
> **Request URL:** /autodiscover/autodiscover.json?@evil.com/owa/&Email=autodiscover/autodiscover.json%3f@evil.com&Protocol=XYZ&FooProtocol=Powershell
> 
> **HTTP Method:** GET
> 
> **User-Agent:** Mozilla/5.0 zgrab/0.x
> 
> **Action:** Blocked
> 
> **Alert Trigger Reason:** Request URL Contains PowerShell


## Analysis
We have the following logs for a connection from 58.237.200.6 to 172.16.20.8. 
![Screen Shot 2022-10-25 at 2 54 28 PM](https://user-images.githubusercontent.com/74877876/197858123-ed4f2dd7-778f-4b0c-8c87-c31fe1e01868.png)

In the last log - Sept. 30, 2022, 7:19 a.m., /autodiscover/autodiscover.json?@evil.com/owa/&Email=autodiscover/autodiscover.json%3f@evil.com&Protocol=XYZ&FooProtocol=Powershell was requested and blocked by the device. 

![Screen Shot 2022-10-25 at 2 56 00 PM](https://user-images.githubusercontent.com/74877876/197858420-d61faf91-32c6-44a8-865a-e8339bd6a638.png)

No browser history, command history, network connections, processes, or emails.

**VirusTotal:** 58.237.200.6 only populates as malicious and community reported as a SSH brute force attack from South Korea. Appending the Powershell to the source IP, https://58.237.200.6/autodiscover/autodiscover.json?@evil.com/owa/&Email=autodiscover/autodiscover.json?@evil.com&Protocol=XYZ&FooProtocol=Powershell: reported as malicious by a few vendors. Not much information.  
**HybridAnalysis:** No results on https://IP/PowerShell, no reults on 58.237.200.6. 
**IBM X-Force:** No results on https://IP/PowerShell, 58.237.200.6 only shows belonging to Korea. 

## Determination
True Positive that 58.237.200.6 tried to remotely execute an attack via PowerShell on 172.16.20.8 but the action was blocked. 


## Helpful Links
- **VirusTotal:** https://www.virustotal.com/gui/url








