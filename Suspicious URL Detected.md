# SOC102 - Proxy - SOC102 - Proxy - Suspicious URL Detected
We received a High alert for a suspicious URL. 

**EventID:** 66
**Event Time:** Feb, 22, 2021, 08:36 PM
**Rule:** SOC102 - Proxy - Suspicious URL Detected
**Level:** Security Analyst
**Source Address:** 172.16.17.150
**Source Hostname:** ChanProd
**Destination Address:** 35.173.160.135
**Destination Hostname:** threatpost.com
**Username:** Chan
**Request URL:** https://threatpost.com/malformed-url-prefix-phishing-attacks-spike-6000/164132/
**User Agent:** Mozilla - Windows
**Device Action:** Allowed

## Analysis:
We'll start off with going to the Log Management and see what the source address pulls. 

Looks like there's a connection to our destination address 35.173.160.135 at Feb, 22, 2021, 08:36 PM using source port 48684. 

![Screenshot 2023-02-13 at 9 20 55 AM](https://user-images.githubusercontent.com/74877876/218483180-7e45ae6a-f3a6-4480-9cbb-182e12ce4335.png)

We do have an allowed connection to the requested URL but it looks like the client used Chrome rather than Firefox. We'll double check the parent process hash: 8b88ebbb05a0e56b7dcc708498c02b3e. 

Going to Endpoint Security: Verified 172.16.17.150 is assigned to ChanProd. No Browser, Command or Network Connections log. 

The Process List doesn't show any processes run at the time of the event. 
![Screenshot 2023-02-13 at 9 26 42 AM](https://user-images.githubusercontent.com/74877876/218484673-0fbb9db8-ddc7-4846-be56-9ce08da3adf1.png)

Using VirusTotal, the Chrome hash from the processes is associated with Outlook.exe: https://www.virustotal.com/gui/file/a02615e156d9d90c2206e68bb597aa97b569a6f7121dd156bafd353d9e910970/details.  
Going back to the Parent Process, 8b88ebbb05a0e56b7dcc708498c02b3e, this is verified by Microsoft as Explorer.exe. 

No emails related to Chan, threatpost, or the date of the event. No threat intelligence already collected on this incident. 

**Let's analyze the URL:**
**VirusTotal:** No known malicious detections on https://threatpost.com/malformed-url-prefix-phishing-attacks-spike-6000/164132/. Serving IP address is the destination address. 

![Screenshot 2023-02-13 at 9 33 25 AM](https://user-images.githubusercontent.com/74877876/218486229-3a692f9c-5547-4f21-9f52-b6f3dcc51128.png)

Based on the related links, this seems to be a blog post. 

**HybridAnalysis:** No known malicious detections. 

**URLscan:** No known malicious detections. Verified destination address 


## Determination:
After looking through the logs, 172.16.17.150 definitely successfully reached threatpost.com. The detection noted Mozilla as the agent, but Chrome was accessed. Analyzing the URL, the post appears to be safe.  

ChanProd successfully reached threatpost.com using Chrome. However, the detection was for Mozilla. Determining to be False Positive as the detected agent was not used. 

## Helpful Resources:
- URL Scan: https://urlscan.io/result/12ae180b-4296-4d94-8244-186c3f8e9c3f/
- HybridAnalysis: https://hybrid-analysis.com/sample/1e249dd6e76416e4fd74909177525fe5b882f9b7848f5dfd81c3506a61364fff
- VirusTotal: https://www.virustotal.com/gui/url/23c1b981a6df184ce99c6d87f20915b156c46fde0ac9fbf319ec48ea8f631aa6
