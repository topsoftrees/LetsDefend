# SOC169 - Possible IDOR Attack Detected
We received a Medium alert for an possible IDOR attack.

**EventID:** 119
**Event Time:** Feb, 28, 2022, 10:48 PM
**Rule:** SOC169 - Possible IDOR Attack Detected
**Level:** Security Analyst
**Hostname:** WebServer1005
**Destination IP Address:** 172.16.17.15
**Source IP Address:** 134.209.118.137
**HTTP Request Method:** POST
**Requested URL:** https://172.16.17.15/get_user_info/
**User-Agent:** Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)
**Alert Trigger Reason:** consecutive requests to the same page
**Device Action:** Allowed


## Analysis
Going right into the logs, we see the following connections. 

![Screenshot 2023-02-17 at 11 50 41 AM](https://user-images.githubusercontent.com/74877876/219714583-08ba045f-10a7-4724-a48e-f82095acb4d6.png)

Verfied 134.209.118.137 reached 172.16.17.15.

Checking each log's details, 134.209.118.137 had an HTTPS POST request to https://172.16.17.15/get_user_info/ using the alerted agent.

![Screenshot 2023-02-17 at 11 53 41 AM](https://user-images.githubusercontent.com/74877876/219715198-ff5ffca3-73d4-45c4-937f-1e7f1d18d531.png)

We know the destination address is a private IP. No need to look into the destination address too much. Let's look at the source address. 

**VirusTotal:**
There are some malicious detections recorded but not specified. Domain is registered in the United States with Digital Ocean. There are community comments for SSH login attempts from this domain. 

![Screenshot 2023-02-17 at 11 59 16 AM](https://user-images.githubusercontent.com/74877876/219716350-0bb158a9-57d6-4527-b857-6a0ba981c15e.png)

I'm going to back up for a second and look into IDOR attacks. From PortSwigger, we learn an IDOR attack is an insecure direct object reference attack. Here are some attributes:
- When an applicaion uses user-supplied input to access objects directly
- Use access controls to mitigate the chance of the attack
- Can be used to access the back-end database or the server-side
- Even though the website can be encrypted with HTTPS, the website might be insecure

Based on the logs, it looks like there was a possible IDOR attack at the date and time of the alert. Let's look at the destination address' log details.
The browser, command, and network connections only show logs from roughly 2 weeks before the time of the alert. From the processes list, nothing seems to be suspicious. 

![Screenshot 2023-02-17 at 12 13 38 PM](https://user-images.githubusercontent.com/74877876/219719629-0103b160-5aab-4d85-94d5-d255242e64ea.png)

The browser processes don't show the time or date they were ran.

Going back to the logs, the POST Parameters sticks out to me. Each one of the logs shows "?user_id=#" where # equals some number. Each log shows 5 different users. I suspect the IDOR attack pulls 5 users. 

![Screenshot 2023-02-17 at 1 47 51 PM](https://user-images.githubusercontent.com/74877876/219758325-b54f809b-6547-4293-8e96-41f083cfede4.png)

![Screenshot 2023-02-17 at 1 48 02 PM](https://user-images.githubusercontent.com/74877876/219758356-211fbf8e-b1cf-45a1-a56d-88dcbf8a7a61.png)

![Screenshot 2023-02-17 at 1 48 12 PM](https://user-images.githubusercontent.com/74877876/219758375-040eab4e-bf8e-42c7-a52d-bea85eb2adea.png)

![Screenshot 2023-02-17 at 1 47 36 PM](https://user-images.githubusercontent.com/74877876/219758274-15f9b883-053a-45f6-a6b1-6bd13f6de611.png)

![Screenshot 2023-02-17 at 1 47 23 PM](https://user-images.githubusercontent.com/74877876/219758237-eb95a6e5-b559-4c99-9350-7906f869d76e.png)

 
## Determination
True positive since 134.209.118.137 was able to successfully view 5 users by accessing the host's (172.16.17.15) insecure object, user_info. 
 
## Helpful Resources
- IDOR Attack: https://portswigger.net/web-security/access-control/idor
 
 
