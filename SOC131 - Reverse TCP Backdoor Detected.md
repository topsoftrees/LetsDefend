# SOC131 - Reverse TCP Backdoor Detected

We received a Medium alert for a reverse TCP backdoor detected. 

**EventID:** 67
**Event Time:** Mar, 01, 2021, 03:15 PM
**Rule:** SOC131 - Reverse TCP Backdoor Detected
**Level:** Security Analyst
**Source Address:** 172.16.17.14
**Source Hostname:** MikeComputer
**File Name:** msi.bat
**File Hash:** 3dc649bc1be6f4881d386e679b7b60c8
**File Size:** 2,12 KB
**Device Action:** Cleaned
**Download (Password:infected):** 3dc649bc1be6f4881d386e679b7b60c8.zip


## Analysis: 
No browser history logged. The only command logged is from 2020, not the time of the alert. 
Network connections from 8/29/2020 around 10:30 to 198.100.45.154 and 67.68.210.95. 

PowerShell was ran on 8/29/2020 around 10:30.
PTD-080120 ZGO-082920.doc was ran on 8/29/2020 around 10:30.
Outlook was ran on 8/29/2020 around 10:30.
Chrome was ran on 8/29/2020 around 10:30.

Verified 198.100.45.154 and 67.68.210.95 were accessed on 8/29/2020 around 10:30. 

**VirusTotal:** Looking up 198.100.45.154 and 67.68.210.95 did not show any detections related to 3dc649bc1be6f4881d386e679b7b60c8. 
None of the processes ran on 8/29/2020 show any relation to the file hash. 
PTD-080120 ZGO-082920.doc and it's hash are related to a well known Trojan that connect to 198.100.45.154 and 67.68.210.95.  

Based on the logs and and verifying PTD-080120 ZGO-082920.doc was accessed and did connect to 198.100.45.154 and 67.68.210.95, there does seem to be an attack on the machine back in 2020. 

Still unsure as to what type of Trojan this is, let's go to HybridAnalysis to look at the behavior. 

**HybridAnalysis:** Looking up PTD-080120 ZGO-082920.doc shows a MITRE detection with the following and connects to the command history.

![Screenshot 2023-02-16 at 8 52 56 AM](https://user-images.githubusercontent.com/74877876/219383264-df6db55f-2942-4492-b54d-4d4840c51655.png)

![Screenshot 2023-02-16 at 8 51 23 AM](https://user-images.githubusercontent.com/74877876/219382878-0d5ae6ef-4aad-46d2-8a4f-f690c5f9d66b.png)

The attack uses encrypting or encoding to obfuscate files or information. 

Looking through the different ATT&CK IDs, there's definitly a command line process that's suspicious. Even though the processes weren't run at the time of the alert, the AV might not have detected the backdoor until it was cleaned at the time of the alert. Since I'm not finding hard evidence of the backdoor existing, let's look at the file.  
![Screenshot 2023-02-16 at 1 12 35 PM](https://user-images.githubusercontent.com/74877876/219452199-a8f70720-1941-482f-b6f6-c3d4596a52b0.png)

From the script, the following sticks out to me: (either to analyze or I'm not sure what it is)
- 81.68.99.93
- __traceback
- threading.Thread
- [(subprocess.Popen(['\\windows\\system32\\cmd.exe']))]
- (socket.socket(socket.AF_INET, socket.SOCK_STREAM))
- lambda
- __import__('os', __g, __g))]][0] for __g['socket']

We're going to analyze each one:
- 81.68.99.93: connects to a malicious Chinese domain
- __traceback: outputs what went wrong
- threading.Thread: run portion of the code until it's complete
- [(subprocess.Popen(['\\windows\\system32\\cmd.exe']))]: run the command line, MITRE ATT&CK
- (socket.socket(socket.AF_INET, socket.SOCK_STREAM)): creates a stream socket
- lambda: anonymous function without a name - suscpious in this case
- __import__('os', __g, __g))]][0] for __g['socket']: accesses the operating system for the network

Based on the file, this definitely seems like a TCP backdoor.

## Determination:
True positive due to the malicious file stored and run back in 2020. The malicious file seems to be a TCP backdoor. AV seems to have detected and cleaned the backdoor at the time of the alert.

## Helpful Resources:
- Lambda: https://www.geeksforgeeks.org/python-lambda-anonymous-functions-filter-map-reduce/
- HybridAnalysis: https://hybrid-analysis.com/
- VirusTotal: https://www.virustotal.com/gui/ip-address/81.68.99.93/relations

