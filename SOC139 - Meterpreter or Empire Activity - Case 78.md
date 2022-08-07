# SOC139 - Meterpreter or Empire Activity

We received a high alert with the following information: 
> **EventID:** 78
> 
> **Event Time:** March 15, 2021, 2:15 p.m.
> 
> **Rule:** SOC139 - Meterpreter or Empire Activity
> 
> **Level:** Security Analyst
> 
> **Source Address:** 172.16.17.55
> 
> **Source Hostname:** Alex - HP
> 
> **File Name:** cobaltstrike_shellcode.exe
> 
> **File Hash:** 24d99ba5654cdf31141c66fd9417b7e0
> 
> **File Size:** 219.00 Kb
> 
> **Device Action:** Allowed
> 
> **Download (Password:infected):** 24d99ba5654cdf31141c66fd9417b7e0.zip

## Analysis

Starting with the source address, a quick look at the time shows nothing close to the time of the alert. We'll still investigate the details of each time. 
Each event shows GET requests from Chrome from BBC, CNN, and searches related to Covid-19. 

<img width="548" alt="Screen Shot 2022-08-07 at 8 25 14 AM" src="https://user-images.githubusercontent.com/74877876/183290428-059b1924-21e5-464d-9c78-04902d9f872f.png">

Looking more in depth to the source IP, back in December 2020, a command was run that appears to possibly be Shell code sending to 10.10.10.10. Since there's a while loop running, I'm guessing a brute force attack. VirusTotal tells us that the community does detect this as malware, however, many vendors have not detected this. 
A few minutes later, a certificate was allowed from a known malicious IP to run one of their services. Here are the details: 

<img width="440" alt="Screen Shot 2022-08-07 at 8 46 31 AM" src="https://user-images.githubusercontent.com/74877876/183291209-1dfb6738-c33a-430e-a112-7adbef268574.png">

Given the echo command, the device is trying to connect to the domain controller. Just before this it appears a user has been added to the domain controller. Given that the device is trying to remain undetected (with the certificate) and connect to the domain controller (where passwords are stored), I'm more confident this is a brute force attack.



Going into the Network Connections, before the possible brute force attack, the host connected to multiple IPs that the VirusTotal community detects as an SSH brute force attack. 

<img width="446" alt="Screen Shot 2022-08-07 at 8 57 38 AM" src="https://user-images.githubusercontent.com/74877876/183291683-7a3cba4e-78b3-40d3-b226-82a6cd88eb56.png">

The final connection, 120.79.181.138, is the moment of the alert. Going into the relations of VirusTotal, one of the related files is called "cobaltstrike_shellcode.bin." Searching for that file - the hash detected is that of the binary file. 

Finally, in the process history, the final process run is the binary with the associated hash. I'm not familiar with the colbaltstrike_shellcode file but doing some googling, it inserts shell code. I'd assume the shell code that's inserting a brute force attack.

## Determination
True Positive.

Detected malware. Code inserted in December 2020 then executed the attack on March 15, 2021 by 120.79.181.138. Recommend changing all passwords and reimaging the machine. 

## Helpful Links
- VirusTotal: https://www.virustotal.com/
