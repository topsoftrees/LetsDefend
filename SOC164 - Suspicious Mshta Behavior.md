# SOC164 - Suspicious Mshta Behavior 
We received a High alert for this potential LOLbin attack:

> **EventID:** 114
> 
> **Event Time:** March 5, 2022, 10:29 a.m.
>
> **Rule:** SOC164 - Suspicious Mshta Behavior
>
> **Level:** Security Analyst
> 
> **Hostname:** Roberto
> 
> **IP Address:** 172.16.17.38
> 
> **Related Binary:** mshta.exe
> 
> **Binary Path:** C:/Windows/System32/mshta.exe
> 
> **Command Line:** C:/Windows/System32/mshta.exe C:/Users/Roberto/Desktop/Ps1.hta
> 
> **MD5 of Ps1.hta:** 6685c433705f558c5535789234db0e5a
> 
> **Alert Trigger Reason:** Low reputation hta file executed via mshta.exe
> 
> **EDR Action:** Allowed


What's an LOLBin? use the preinstalled apps (Windows binaries) to spread malware - "living off the land"  

## Analysis
We see that 172.16.17.38 connects to 193.142.58.23. The requested URL is http://193.142.58.23/Server.txt with HTTP response 404. 

![Screen Shot 2022-10-25 at 9 40 03 AM](https://user-images.githubusercontent.com/74877876/197788907-b74cac67-0b1c-45f6-9a86-5ec2033d7559.png)

**VirusTotal:** We can see http://193.142.58.23/Server.txt has 193.142.58.23 as a serving address and HTTP response 404. There have been many reports of maliciousness. 

![Screen Shot 2022-10-25 at 9 42 10 AM](https://user-images.githubusercontent.com/74877876/197789463-0107f4f4-8582-48b9-a01b-6482a402f310.png)

Server.txt should have a different hash as Ps1.hta.

Ps1.hta doesn't populate any results in VirusTotal but 6685c433705f558c5535789234db0e5a does. 6685c433705f558c5535789234db0e5a is known as Ps1.txt.
![Screen Shot 2022-10-25 at 10 02 31 AM](https://user-images.githubusercontent.com/74877876/197794665-a4706463-e972-4d31-bb29-e00038a02cc2.png)

There were no web browser searches on the day and time of the alert. The previous few searches that were made in March don't appear to be related to malcious content. We do have the following  executed, http://193.142.58.23/Server.txt is accessed.
![Screen Shot 2022-10-25 at 10 38 18 AM](https://user-images.githubusercontent.com/74877876/197803823-f3ddfaf5-a35e-4397-a30e-d262777980e8.png)

We see in process history that 'C:/Windows/System32/mshta.exe C:/Users/roberto/Desktop/Ps1.hta' was run with a hash associated with mshta.exe. The hash of mshta.exe is verified by Microsoft. MSHTA.EXE was used to execute PowerShell script Ps1.hta to access http://193.142.58.23/Server.txt.  

The suscpicious activity was execution of Ps1.hta by the user (we know it's not malware because LOLbin uses preinstalled software.)

## Determination
True Positive, confirmed LOLbin behavior via MSHTA.EXE and Powershell, executed by the user. MSHTA.EXE was used to execute PowerShell script Ps1.hta to access http://193.142.58.23/Server.txt and received expected malicious HTTP response 404.

## Helpful Links
- **LOLbins:** https://www.cynet.com/attack-techniques-hands-on/what-are-lolbins-and-how-do-attackers-use-them-in-fileless-attacks/
- **VirusTotal:** https://www.virustotal.com/gui

