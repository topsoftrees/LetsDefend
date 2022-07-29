# SOC134 - Suspicious WMI Activity - Case 81
We received a high alert for the following information:
> 	**EventID:** 81
> 	
> 	**Event Time:** March 15, 2021, 10:57 p.m.
> 	
> 	**Rule:** SOC134 - Suspicious WMI Activity
> 	
> 	**Level:** Security Analyst
> 	
> 	**Source Address:** 172.16.20.3
> 	
> 	**Source Hostname:** Exchange Server
> 	
> 	**File Name:** lunch.exe
> 	
> 	**File Hash:** f2b7074e1543720a9a98fda660e02688
> 	
> 	**File Size:** 6.66 Mb
> 	
> 	**Device Action:** Cleaned
> 	
> 	**Download (Password:infected):** f2b7074e1543720a9a98fda660e02688.zip

## Analysis 
Starting with searching the hash in VirusTotal, it's been heavily reported as being a Trojan with a binary name of lunch. There's been no executable reported with the name lunch.exe. This doesn't mean anything as we can rely on the hash. 

![Screen Shot 2022-07-29 at 12 36 21 PM](https://user-images.githubusercontent.com/74877876/181804817-e9494a9d-6498-4822-9b63-85301db0589c.png)

So we know the hash is of malware, but we can't call it a day because we need to prove it exists on this machine. 

Going to Log Management and doing a search for the source address reports no results. Going to Endpoint Security pulls up the following information: 

![Screen Shot 2022-07-29 at 12 40 26 PM](https://user-images.githubusercontent.com/74877876/181805439-9524480a-9d75-4afe-a31a-db64fee2b6dc.png)

Seeing as the last login was before the alert, my first instinct says it's a false positive but let's do some further analysis.

- Browser History: there is no browser
- Command History: net was ran. I'm not familiar with this tool so I had some digging to do. Because the date of the commands, I'm still led to believe it's a false positive.

Net is a tool for Samba and remote CIFS servers. I'm not familiar with Samba servers. They appear to commonly be file servers for Linux and Windows. Below are the commands with my annotations:

> 2020-10-10 10:29:01: cls # another version of clear
> 
> 2020-10-10 10:29:02: net user # assuming looking at the users with access to file shares
> 
> 2020-10-10 10:29:03: net user backupUser # assuming selecting a user
> 
> 2020-10-10 10:29:04: net localgroup backupGroup backupUser /add # assuming creating an alias called "backupGroup" for "backupUser" per the net link under the Helpful Websites.


- Network Connections: Agent Down
- Process List: Verified all hashes are reported in VirusTotal and HybridAnalysis as not malware. Most were verified by Microsoft. The final process was the clear Event Log via PowerShell

Searching for the hash in Threat Intelligence does not provide any entries. 


## Determination 
Given the time of the alert and no known source of a logic bomb, reporting as a false positve. The file is malcious but it was not accessed.

I failed the case, it's actually a true positive. My questions- how did it get onto the Exchange Server? Does Device Action mean it's been cleaned by an AV making any info on the Exchange Server ambiguous?
Went to https://app.any.run/tasks/6ed0a328-8f35-41b0-8980-3e23a23f3bea/ to see the sandbox version of the hash, it shuts off the machine. I've reached out to other user's of LetsDefend for clarification on this. Is it a true positive because an AV would get rid of any trace of it and not because we could find a trace of it? 

## Helpful Websites
- net: https://linux.die.net/man/8/net
- Samba: https://ubuntu.com/server/docs/samba-file-server
