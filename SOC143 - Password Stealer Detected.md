# SOC143 - Password Stealer Detected

We received a Medium alert for a real cyber attack for a password stealer. 

**EventID:** 90
**Event Time:** Apr, 26, 2021, 11:03 PM
**Rule:** SOC143 - Password Stealer Detected
**Level:** Security Analyst
**SMTP Address:** 180.76.101.229
**Source Address:** bill@microsoft.com
**Destination Address:** ellie@letsdefend.io
**E-mail Subject:** .
**Device Action:** Allowed

## Analysis:
Let's start of by looking at the email. 

![Screenshot 2023-02-14 at 7 51 44 AM](https://user-images.githubusercontent.com/74877876/218744262-5ce1eab1-ce47-472f-845e-b687c4d3236e.png)

The email was received so it was Allowed. According to the email, the date, source and destination address check out. LetsDefend.io doesn't provide the email header so that's out.

**MxToolbox:** Looked up the SMTP address, no DNS records found. Looked up microsoft.com domain, 
![Screenshot 2023-02-14 at 7 59 29 AM](https://user-images.githubusercontent.com/74877876/218745982-3047570a-ab9a-4a18-88a1-b4fa9794b9b2.png)

So far, this email looks spoofed. 

**VirusTotal:** SMTP address is owned by Beijing Baidu Netcom Science and Technology Co., Ltd and the only related file is called Kane.exe. 
Based on the DNS record, the email is definitely spoofed. 

Looking at the file in analysis tools: bd05664f01205fa90774f42468a8743a.zip. Isolated the file and unzipped it. 
VirusTotal: The file pulls 15 malicious detections for phishing or a trojan. 

![Screenshot 2023-02-14 at 8 08 54 AM](https://user-images.githubusercontent.com/74877876/218748064-dbe117e7-7a95-42a3-97c7-67bbdf9e618a.png)

MD5: bd05664f01205fa90774f42468a8743a
SHA-256: 58c45547bccce5eb16d84bae13eb0c2813ffe03e34eae622b65468a6b289ca37
Another name: Ellie@letsdefend.io_63963965Application.HTML

**HybridAnalysis:** looking up the MD5 hash shows a 100/100 threat score. 

![Screenshot 2023-02-14 at 8 13 15 AM](https://user-images.githubusercontent.com/74877876/218749151-f290b6e0-e16f-43a6-a072-49ba2194bc60.png)

The Falcon Sandbox detects the file as a Trojan. 
Looking through the IOCs, 
- The file sends traffic to Google and Dropbox. 
- The file reached browser user data. Most likely sensitive information. This is a huge red flag for a password stealer to me. 

![Screenshot 2023-02-14 at 8 16 40 AM](https://user-images.githubusercontent.com/74877876/218749885-8f75dc2e-3a01-4b4c-96ee-eb28039a28d8.png)

I've written a Medium article on MITRE: https://medium.com/@saravra/understanding-cybersecurity-attacks-b11772d37a2. The attack is associated with ATT&CK ID T1005 which the threat can search for file systems, config files, of local databases to find sensitive information. 

- The file accesses temporary directories. Many of them appear to be related to money or validate the credentials. 
![Screenshot 2023-02-14 at 8 21 22 AM](https://user-images.githubusercontent.com/74877876/218750873-6c195308-b877-4c0b-a3ad-68d39269fd2c.png)

Looking through sandbox's processes that msedge executed, "--display-capture-permissions-policy-allowed" sticks out to me as a password stealer. 

Going back to the initial detections from HybridAnalysis, there are similar detections with Internet Explorer. 

## Determination:
Since a policy to allow capturing of permissions was executed, DropBox was accessed, user browser data was accessed, and email source was spoofed determining this to be a true positive. 


## Helpful Resources:
- MxToolbox: https://mxtoolbox.com/SuperTool.aspx?action=a%3a180.76.101.229&run=toolpage
- HybridAnalysis: https://hybrid-analysis.com/sample/58c45547bccce5eb16d84bae13eb0c2813ffe03e34eae622b65468a6b289ca37
- VirusTotal: https://www.virustotal.com/gui/file/58c45547bccce5eb16d84bae13eb0c2813ffe03e34eae622b65468a6b289ca37/details

