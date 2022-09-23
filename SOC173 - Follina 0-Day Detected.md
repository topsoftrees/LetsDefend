# SOC173 - Follina 0-Day Detected - Case 123 
 This alert was generated from a real malware attack: Microsoft Windows Support Diagnostic Tool (MSDT) Remote Code Execution Vulnerability, CVE-2022-30190.
 
 We received a Medium alert for the following Malware attack:
 > **EventID:** 123
 > 
 > **Event Time:** June 2, 2022, 3:22 p.m.
 > 
 > **Rule:** SOC173 - Follina 0-Day Detected
 > 
 > **Level:** Security Analyst
 > 
 > **Source Address:** 172.16.17.39
 > 
 > **Hostname:** JonasPRD
 > 
 > **File Name:** 05-2022-0438.doc
 > 
 > **File Hash:** 52945af1def85b171870b31fa4782e52
 > 
 > **File Size:** 10.01 Kb
 > 
 > **AV Action:** Allowed
 > 
 > **Alert Trigger Reason:** msdt.exe executed after Office document
 > 
 > **Download (Password:infected):** 05-2022-0438.doc.zip

## Analysis 
Log Management: searching for 172.16.17.39 results in many connections to 141.105.65.149. VirusTotal detects 141.105.65.149 as related to xmlformats 

Searching for the file in HybridAnalysis relates the MD5 hash to a SHA-256 file hash of a malicious file associated with cve-2022-30190. The file is also related to xmlformats. 

MD5: 52945af1def85b171870b31fa4782e52.
SHA-256: 4a24048f81afbe9fb62e7a6a49adbd1faf41f266b5f9feecdceb567aec096784. 
CVE-2022-30190: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-30190

![Screen Shot 2022-09-23 at 12 11 02 PM](https://user-images.githubusercontent.com/74877876/192005296-3df5fbb6-47d5-47b9-b7da-b7a65e102e25.png)

![Screen Shot 2022-09-23 at 12 54 13 PM](https://user-images.githubusercontent.com/74877876/192012731-14cb174b-f4cd-48ca-b3ac-62c93f7766d7.png)

The VirusTotal Relations of 141.105.65.149 show relations to 05-2022-0438.doc.zip and 4a24048f81afbe9fb62e7a6a49adbd1faf41f266b5f9feecdceb567aec096784.zip. Notice the latter file is the SHA-256 hash of the malicious file. 

From the Browser History and Network Connections, nothing appears to connect at the time of the alert. Network Connections to the following sites. Browser History doesn't appear to be related to the xmlformats site.
- 02.06.2022 02:03: 172.217.169.142
- 02.06.2022 04:01: 127.0.0.1
- 02.06.2022 05:02: 99.83.154.118
- 02.06.2022 19:48: 157.240.238.14

![Screen Shot 2022-09-23 at 12 38 53 PM](https://user-images.githubusercontent.com/74877876/192010160-febbc420-7a4f-43e5-9007-bcdcbb80afcf.png)

However, at the time of the alert, it looks like a msdt task was killed then a rar file was accessed. 

![Screen Shot 2022-09-23 at 12 23 54 PM](https://user-images.githubusercontent.com/74877876/192007584-f07505f4-824b-4a38-9e87-f3c1950ff706.png)

I'm curious as to how this got on the device. We can clearly see it received it from 141.105.65.149 but that IP doesn't show in the network connections or the Browser Connections. This next part I feel like is overkill but I'll go ahead and include it. 

> We can see that the msdt file was accessed but I'm not sure what the string means in the msdt.exe command: 
> 
> ![Screen Shot 2022-09-23 at 12 49 00 PM](https://user-images.githubusercontent.com/74877876/192011861-c96a3b17-654b-459e-aa5d-e120dd3bd330.png)
> 
> This next part required some searching online of decoding. I came across CyberChef and try decoding base64. I know it's not hex and not regex. Decoded the string that I don't know that was ran for msdt.exe:
> 
> ![Screen Shot 2022-09-23 at 1 04 42 PM](https://user-images.githubusercontent.com/74877876/192014324-d53a50ea-088e-4cbd-a503-d8608bb3a87d.png)
> 
> Here is the result. You'll notice that it's the command that's accesses the rar file. 
https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=SkdOdFpDQTlJQ0pqT2x4M2FXNWtiM2R6WEhONWMzUmxiVE15WEdOdFpDNWxlR1VpTzFOMFlYSjBMVkJ5YjJObGMzTWdKR050WkNBdGQybHVaRzkzYzNSNWJHVWdhR2xrWkdWdUlDMUJjbWQxYldWdWRFeHBjM1FnSWk5aklIUmhjMnRyYVd4c0lDOW1JQzlwYlNCdGMyUjBMbVY0WlNJN1UzUmhjblF0VUhKdlkyVnpjeUFrWTIxa0lDMTNhVzVrYjNkemRIbHNaU0JvYVdSa1pXNGdMVUZ5WjNWdFpXNTBUR2x6ZENBaUwyTWdZMlFnUXpwY2RYTmxjbk5jY0hWaWJHbGpYQ1ltWm05eUlDOXlJQ1YwWlcxd0pTQWxhU0JwYmlBb01EVXRNakF5TWkwd05ETTRMbkpoY2lrZ1pHOGdZMjl3ZVNBbGFTQXhMbkpoY2lBdmVTWW1abWx1WkhOMGNpQlVWazVFVW1kQlFVRkJJREV1Y21GeVBqRXVkQ1ltWTJWeWRIVjBhV3dnTFdSbFkyOWtaU0F4TG5RZ01TNWpJQ1ltWlhod1lXNWtJREV1WXlBdFJqb3FJQzRtSm5KbllpNWxlR1VpT3c9PQo

Since I was confused about how the file got on here and it clearly looks like the remote code execution is on here, I went to the Inbox for a final look. Looks like and ria.ru email sent the file to Jonas. 

![Screen Shot 2022-09-23 at 1 15 57 PM](https://user-images.githubusercontent.com/74877876/192018532-23b856c4-5a04-4aaa-a088-443902f5e9a2.png)

## Determination
True Positive as the file was emailed to the user which connects to a malicious IP related to cve-2022-30190.

## Helpful Links
