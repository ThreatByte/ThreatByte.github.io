# Scoping Notes
(＞ω＜) ♡ Welcome to Waifu University ♡ (＞ω＜)

Waifu University's cyber team has called you after their IT teams reported a number of servers with files that aren't opening and have a strange extension.

On your scoping call, the victim also said they had identified a ransom note stating their data has been stolen. When asked about any earlier signs, the victim mentioned some strange, failed login activity early in March 2024 in their Entra ID, but wasn't of concern at the time...

Ransomware will typically avoid system files to not cause crashes in the system, which also happens to be where a lot of forensic evidence is! You have been provided triage images of the hosts and log exports from the relevant systems.

The Waifu University team took triage collections from the affected hosts using the account WAIFU\kscanlan6 at approximately 2024-03-07 05:00:00 UTC. Consider activity after this point related to the response.
-------------------------------------------------------------------------------
## Scoping the Incident
What was the domain the threat actor has requested the victim to visit in order to further communications?

To find this information i extracted all the Triage Images and looked through them to find any ReadMe files or Recovery Files. Typically TA target DC because they control Active Directory which would allow you to reset passwords, change permissions, deploy malware to users, and change policies.

rfosusl6qdm4zhoqbqnjxaloprld2qz35u77h4aap46rhwkouejsooqd.onion

<img width="1921" height="915" alt="Downloads2" src="https://github.com/user-attachments/assets/b6c714b5-9017-4347-96cd-f0c2ba315bb2" />


-------------------------------------------------------------------------------
What was the file name of the ransom note left behind by the ransomware?

The file name that was left behind from the threat actor was "RECOVER-kh1ftzx-FILES.txt".  We know this because the threat actor has requested to victim to go to the .onion url to further communications.

<img width="875" height="470" alt="Downloads" src="https://github.com/user-attachments/assets/9bc15440-bfd2-4ad7-9a59-6ec8a0fedbcb" />

--------------------------------------------------------------------------------------------------------------------
What was the file extension the ransomware added to encrypted files?

The file extension the ransomware added to the encrypted files was .kh1ftzx.

-------------------------------------------------------------------------------
## Initial Access via Entra ID

Our security team mentioned that there was a number of failed logons to the victims Identity Provider (Entra ID).

What was the full user agent string that was responsible for these attempts?

Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.0.0 Safari/537.36

I never used Entra ID so i did some googling and found this link: https://learn.microsoft.com/en-us/entra/identity-platform/reference-error-codes.

The link shows you references to Entra ID authentication and error codes. The result type code i used was "50126". The code means InvalidUserNameOrPassword - Error validating credentials due to invalid username or password. The user didn't enter the right credentials. Expect to see some number of these errors in your logs due to users making mistakes.

There was only one invalid logon attempt on 2024-02-28 12:00 compared to thirty invalid logon attempts on 2024-03-03 00:00:00. 

![[Lab 1.png]]  

Elastic Query : azure.activitylogs.result_type: "50126"

The user we are going to be focusing on is Kurtis Scanlan based off of the Scoping Notes. There was four failed login attempts. 

![[Lab1.1.png]] 

Elastic Query: azure.activitylogs.result_type: "50126" and azure.activitylogs.identity_name: Kurtis Scanlan. To view the userAgent click on the cell.

![[Pasted image 20250831084206.png]]
--------------------------------------------------------------------------------------------------------------------
What was the cloud provider the threat actor used to proxy their requests? Please use the acronym.

AWS

To get the information regarding the TA proxying their requests enter the IP address in ipinfo.io. Use the source.ip address to determine what cloud provider the threat actor was using.

![[Pasted image 20250831084808.png]]

https://ipinfo.io/3.146.44.67?lookup_source=search-bar

-------------------------------------------------------------------------------
How many unique users did the threat actor attempt to authenticate with?

The number of unique users that the threat actor attempted to authenticate with was eight. 

![[Pasted image 20250831085603.png]]

-------------------------------------------------------------------------------
What was the User Principal Name (UPN) of the user the threat actor succeeded in accessing?
ivanderplas1@waifu.phd

![[Pasted image 20250831094121.png]] 

Elastic Query: azure.activitylogs.identity_name:* azure.activitylogs.realDescription: Strong Authentication 

-------------------------------------------------------------------------------
What was the most likely method the threat actor was able to authenticate to the VPN that was protected by MFA?

![[Lab 1.2.png]] 
Elastic Query: azure.activitylogs.identity_name : * and azure.activitylogs.resultDescription : "Authentication failed during strong authentication request." 

-------------------------------------------------------------------------------

What was the IP that successfully logged into the environment?
207.246.70.192

![[Lab 1.3.png]] 
Elastic Query: azure.activitylogs.identity_name : * and azure.activitylogs.identity_name: Ignazio Vanderplas

-------------------------------------------------------------------------------
What was the SSH fingerprint for the IP?

Note: Make sure to look for historical info around the time of the intrusion as IP addresses may frequently change owners.

97:2e:5d:5e:ca:d1:15:a9:51:ed:8b:0e:55:f1:6a:ee

![[Lab 1.4.png]] 

-------------------------------------------------------------------------------
## Breaching the University

What was the host name the threat actor was able to first access once in the network (also known as the beachhead host)?

To find the host name of the threat actor you want to look for any successful logons that are in scope of March 3rd 2024. The host/name that was targeted was cc-jmp-01 on March 3rd 2024 13:37:32.

![[Lab 1.5.png]] 

Elastic Query: event.code:4624 and winlog.event_data.TargetUserName: "ivanderplas1"

Confirmation in Timeline explorer: 

![[Lab 1.6.png]]
![[Lab 1.7.png]] 

What was the hostname of the threat actor's device used to get into the network?

To find the target hostname you can look at the Payload in Timeline explorer. The name would be the "WorkstationName"

![[Lab 1.8.png]]
-------------------------------------------------------------------------------
What did the threat actor search for via a Web Browser from the initial beachhead host?

To find out what the threat actor searched for via Web Browser you can use a tool called Nirsoft BrowsingHistoryView. The threat actor searched for "Whatismyip"

![[Lab 1.9.png]] 
--------------------------------------------------------------------------------------------------------------------
The initial compromised user created a PowerShell process on the beachhead host. What domain did the process make a DNS query for?

_Format: domain.com (do not include subdomains)_

To find this you can search up the event id 22. 

![[Lab 1.10.png]]
![[Lab 1.11.png]]
![[Lab 1.12.png]]
-------------------------------------------------------------------------------
What was the Github repo URL the threat actor downloaded a tool from on the beachhead host?

_Format: [https://github.com/username/repository](https://github.com/username/repository)_ 

![[Lab 1.13.png]]
To find this information you can look at the path "C:\Users\ivanderplas1\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline"

--------------------------------------------------------------------------------------------------------------------
## Privilege Escalation

One of our analysts was able to identify the threat actor was querying service information on the beachhead host. Shortly after they seemed to have administrator access.

What was the name of the service the threat actor targeted?

![[Lab 1.14.png]]

![[Lab 1.17.png]]
--------------------------------------------------------------------------------------------------------------------
What was the MITRE ATT&CK technique that the threat actor used to escalate privilege?

T1574.009

https://dmcxblue.gitbook.io/red-team-notes/persistence/path-interception
https://attack.mitre.org/techniques/T1574/009/

-------------------------------------------------------------------------------
What was the name of the binary the service spawned?

waifu.exe

![[Lab 1.15.png]]
-------------------------------------------------------------------------------
What was the SHA1 of this binary?

![[Lab 1.16.png]]
--------------------------------------------------------------------------------------------------------------------
## Remote Access

What did the threat actor successfully run as NT AUTHORITY\SYSTEM under this service?

ScreenConnect

![[Lab 1.18.png]]
--------------------------------------------------------------------------------------------------------------------
What was the domain the remote access tool communicated to?
![[Lab 1.19.png]]
To get the QueryName look for EventLog 22 and search for screenconnect.

-------------------------------------------------------------------------------
Once the threat actor was able to escalate their privileges on the beachhead host, they ran an interesting payload.

It looks like a threat actor replaced an existing .exe with some type of malware.

What is the full path of the exe?

![[Lab 1.20.png]]

-------------------------------------------------------------------------------
There is more to this binary than it seems... The sysmon event logs didn't record any network activity but it may be due to the limitation of the configuration.

We ran a process dump and provided it in the file malicious_process.zip

The password of the zip is the process name of the binary (example: explorer.exe).

What was the IP the beacon talks to?

207.246.70.192

![[Lab 1.21.png]]

-------------------------------------------------------------------------------
Based on the same process dump, can you identify the domain the beacon uses in the host header?

screenconnect.dev

![[Lab 1.21 1.png]]
--------------------------------------------------------------------------------------------------------------------
## Accessing Volume Shadow Copies

What was the timestamp of the command that the threat actor used to access a volume shadow copy of the beachhead host?

_Format: YYYY-MM-DD HH:MM:SS_

2024-03-05 21:23:17

You can use event id 4688 to find the volume shadow copy program that was executed.

![[Lab 1.22.png]]
-------------------------------------------------------------------------------
What was the file name that was responsible for running the volume shadow copy command?

![[Lab 1.23.png]]
2024-03-05 21:23:17

-------------------------------------------------------------------------------
## Looking Around The Network

The threat actor was able to open a file that was supposed to be in a hidden share from the beachhead host.

What was the name of the file?

_*Note: Just have one extension not two in the answer_

you-cant-see-this-cause-I-am-good-at-NTFS-permissions.txt.txt

![[Lab 1.30.png]]
-------------------------------------------------------------------------------
What discovery tool did the threat actor create through their beacon?

_Format: Mimikatz.exe_

![[Lab 1.13.png]] 

-------------------------------------------------------------------------------
Was the threat actor successful in executing this tool?

NO events for 4688.

-------------------------------------------------------------------------------
What group did the malicious process enumerate on the beachhead host?

_Format: Remote Desktop Users_

Administrators

![[Lab 1.25.png]]
-------------------------------------------------------------------------------
## Domain Dominance

What was the name of the malicious service that the threat actor attempted to install on the Domain Controller?

8628f7b

![[Lab 1.26.png]]

-------------------------------------------------------------------------------

What was the possible operating system distribution the threat actor is using?

parrot

![[Lab 1.27.png]]

-------------------------------------------------------------------------------

What was the name of the DLL installed onto one of the domain controller's processes to monitor for credentials?


![[Lab 1.28.png]]

![[Lab 1.29.png]]

Location of PTASpy.dll file.

-------------------------------------------------------------------------------

What was the file name of the process that this DLL was injected to?


AzureADConnectAuthenticationAgentService.exe

![[Lab 1.28 1.png]]

-------------------------------------------------------------------------------

What is the likely user that had their credentials recorded by the DLL.

The user account whose credentials was most likely stolen was cpecht7@waifu.phd

-------------------------------------------------------------------------------
## Accessing the Good Stuff

Which account did the threat actor access the SQL server with via RDP?

cc-admin

![[Lab 1.31.png]]

-------------------------------------------------------------------------------

The University admins noticed a strange file in the documents folder of the admin user for the SQL server which was created during the intrusion. What was the earliest MFT Entry ID for the file name in this path?

![[Lab 1.34.png]]

-------------------------------------------------------------------------------
## Release the Ransomware UwU

What was the name of the ransomware binary?

print64.exe

![[Lab 1.35.png]]

![[Lab 1.36.png]]


What was the access token provided to the binary to decrypt the payload?

uwuwuwuwuwuwuw

![[Lab 1.36 1.png]]
