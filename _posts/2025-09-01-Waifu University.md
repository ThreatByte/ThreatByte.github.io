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

<img width="1918" height="916" alt="Lab 1" src="https://github.com/user-attachments/assets/28bff0d9-8bdf-4501-874a-da81f77a5739" />

Elastic Query : azure.activitylogs.result_type: "50126"

The user we are going to be focusing on is Kurtis Scanlan based off of the Scoping Notes. There was four failed login attempts. 

<img width="1919" height="914" alt="Lab1 1" src="https://github.com/user-attachments/assets/a349f6db-7737-43c4-885b-6811d69924e1" />

Elastic Query: azure.activitylogs.result_type: "50126" and azure.activitylogs.identity_name: Kurtis Scanlan. To view the userAgent click on the cell.

<img width="383" height="288" alt="Pasted image 20250831084206 1 1" src="https://github.com/user-attachments/assets/7d97d877-cbff-4453-b802-961d2260d09d" />

--------------------------------------------------------------------------------------------------------------------
What was the cloud provider the threat actor used to proxy their requests? Please use the acronym.

AWS

To get the information regarding the TA proxying their requests enter the IP address in ipinfo.io. Use the source.ip address to determine what cloud provider the threat actor was using.

<img width="386" height="288" alt="Pasted image 20250831084808" src="https://github.com/user-attachments/assets/f2e54080-161c-4311-8af3-6c4ea7a29ff3" />


https://ipinfo.io/3.146.44.67?lookup_source=search-bar

-------------------------------------------------------------------------------
How many unique users did the threat actor attempt to authenticate with?

The number of unique users that the threat actor attempted to authenticate with was eight. 

<img width="1606" height="313" alt="Pasted image 20250831085603" src="https://github.com/user-attachments/assets/c75c7d35-18da-4953-9141-dfb4f71aa88c" />

-------------------------------------------------------------------------------
What was the User Principal Name (UPN) of the user the threat actor succeeded in accessing?
ivanderplas1@waifu.phd

<img width="1918" height="916" alt="Pasted image 20250831094121" src="https://github.com/user-attachments/assets/17785fa2-2c7c-47f6-bb0a-3e5dafc4daaa" />


Elastic Query: azure.activitylogs.identity_name:* azure.activitylogs.realDescription: Strong Authentication 

-------------------------------------------------------------------------------
What was the most likely method the threat actor was able to authenticate to the VPN that was protected by MFA?

<img width="1922" height="918" alt="Lab 1 2" src="https://github.com/user-attachments/assets/1c618ec5-2060-4a57-aaa3-82f8c1526522" />

Elastic Query: azure.activitylogs.identity_name : * and azure.activitylogs.resultDescription : "Authentication failed during strong authentication request." 

-------------------------------------------------------------------------------

What was the IP that successfully logged into the environment?
207.246.70.192

<img width="1922" height="916" alt="Lab 1 3" src="https://github.com/user-attachments/assets/c366c955-e0c0-4cb5-a254-154108068d8d" />

Elastic Query: azure.activitylogs.identity_name : * and azure.activitylogs.identity_name: Ignazio Vanderplas

-------------------------------------------------------------------------------
What was the SSH fingerprint for the IP?

Note: Make sure to look for historical info around the time of the intrusion as IP addresses may frequently change owners.

97:2e:5d:5e:ca:d1:15:a9:51:ed:8b:0e:55:f1:6a:ee

<img width="1920" height="960" alt="Lab 1 4" src="https://github.com/user-attachments/assets/0fe39467-9257-42b2-9f38-08e040634eca" />


-------------------------------------------------------------------------------
## Breaching the University

What was the host name the threat actor was able to first access once in the network (also known as the beachhead host)?

To find the host name of the threat actor you want to look for any successful logons that are in scope of March 3rd 2024. The host/name that was targeted was cc-jmp-01 on March 3rd 2024 13:37:32.

<img width="1925" height="922" alt="Lab 1 5" src="https://github.com/user-attachments/assets/5a3656c9-5f7d-42da-9cf1-4c76e5375b88" />


Elastic Query: event.code:4624 and winlog.event_data.TargetUserName: "ivanderplas1"

Confirmation in Timeline explorer: 

<img width="1874" height="30" alt="Lab 1 6" src="https://github.com/user-attachments/assets/6fc65c26-5f19-48cc-b614-2a7045d149d3" />
<img width="1877" height="26" alt="Lab 1 7" src="https://github.com/user-attachments/assets/c8813b8d-168f-4423-9b94-81b288590c98" />


What was the hostname of the threat actor's device used to get into the network?
<img width="445" height="27" alt="Lab 1 8" src="https://github.com/user-attachments/assets/92931d7b-8cae-480a-8a3f-a2e8052ddc19" />

To find the target hostname you can look at the Payload in Timeline explorer. The name would be the "WorkstationName"


-------------------------------------------------------------------------------
What did the threat actor search for via a Web Browser from the initial beachhead host?

To find out what the threat actor searched for via Web Browser you can use a tool called Nirsoft BrowsingHistoryView. The threat actor searched for "Whatismyip"

<img width="725" height="82" alt="Lab 1 9" src="https://github.com/user-attachments/assets/34eaf270-20fd-4cdb-b60a-bbc96830d75a" />

--------------------------------------------------------------------------------------------------------------------
The initial compromised user created a PowerShell process on the beachhead host. What domain did the process make a DNS query for?

_Format: domain.com (do not include subdomains)_

To find this you can search up the event id 22. 

<img width="1894" height="31" alt="Lab 1 10" src="https://github.com/user-attachments/assets/0ca8eb46-44c1-46cd-8489-c3a79c65b25a" />
<img width="1000" height="32" alt="Lab 1 11" src="https://github.com/user-attachments/assets/5f0b6439-ec00-47e7-b3ea-81529f5a584a" />
<img width="1255" height="27" alt="Lab 1 12" src="https://github.com/user-attachments/assets/8630b9de-fdc6-4fe2-beae-a9e6bbc16895" />

-------------------------------------------------------------------------------
What was the Github repo URL the threat actor downloaded a tool from on the beachhead host?

_Format: [https://github.com/username/repository](https://github.com/username/repository)_ 

<img width="1923" height="924" alt="Lab 1 13" src="https://github.com/user-attachments/assets/02ceae54-c580-4ff8-a50f-997972c21fde" />

To find this information you can look at the path "C:\Users\ivanderplas1\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline"

--------------------------------------------------------------------------------------------------------------------
## Privilege Escalation

One of our analysts was able to identify the threat actor was querying service information on the beachhead host. Shortly after they seemed to have administrator access.

What was the name of the service the threat actor targeted?


<img width="1922" height="919" alt="Lab 1 14" src="https://github.com/user-attachments/assets/c5ebca1f-8c5f-47cf-8ea4-6f53240ea8b9" />
<img width="1618" height="258" alt="Lab 1 17" src="https://github.com/user-attachments/assets/55d3ad92-8112-4efe-9a40-751f3673ca96" />


--------------------------------------------------------------------------------------------------------------------
What was the MITRE ATT&CK technique that the threat actor used to escalate privilege?

T1574.009

https://dmcxblue.gitbook.io/red-team-notes/persistence/path-interception
https://attack.mitre.org/techniques/T1574/009/

-------------------------------------------------------------------------------
What was the name of the binary the service spawned?

waifu.exe

<img width="1921" height="920" alt="Lab 1 15" src="https://github.com/user-attachments/assets/b9aa348a-f51c-4803-a981-87a587a87f11" />

-------------------------------------------------------------------------------
What was the SHA1 of this binary?

<img width="979" height="178" alt="Lab 1 16" src="https://github.com/user-attachments/assets/c7663396-925f-412d-934b-1c3c30ceed01" />

--------------------------------------------------------------------------------------------------------------------
## Remote Access

What did the threat actor successfully run as NT AUTHORITY\SYSTEM under this service?

ScreenConnect

<img width="395" height="385" alt="Lab 1 18" src="https://github.com/user-attachments/assets/076caead-b6c9-4e26-871a-58a6f6bdfb03" />

--------------------------------------------------------------------------------------------------------------------
What was the domain the remote access tool communicated to?
<img width="813" height="480" alt="Lab 1 19" src="https://github.com/user-attachments/assets/6bd48baa-7f82-4107-b12f-d76bb47b9d9d" />

To get the QueryName look for EventLog 22 and search for screenconnect.

-------------------------------------------------------------------------------
Once the threat actor was able to escalate their privileges on the beachhead host, they ran an interesting payload.

It looks like a threat actor replaced an existing .exe with some type of malware.

What is the full path of the exe?

<img width="1883" height="30" alt="Lab 1 20" src="https://github.com/user-attachments/assets/17a3d797-a41d-46a1-8c39-545bd5b14a10" />


-------------------------------------------------------------------------------
There is more to this binary than it seems... The sysmon event logs didn't record any network activity but it may be due to the limitation of the configuration.

We ran a process dump and provided it in the file malicious_process.zip

The password of the zip is the process name of the binary (example: explorer.exe).

What was the IP the beacon talks to?

207.246.70.192


<img width="1914" height="309" alt="Lab 1 21" src="https://github.com/user-attachments/assets/25ae8179-8784-4fe6-b2cc-e4b487e25e79" />

-------------------------------------------------------------------------------
Based on the same process dump, can you identify the domain the beacon uses in the host header?

screenconnect.dev
<img width="1914" height="309" alt="Lab 1 21 1" src="https://github.com/user-attachments/assets/954f232d-846a-4f8a-9eda-ce798cef9ac4" />


--------------------------------------------------------------------------------------------------------------------
## Accessing Volume Shadow Copies

What was the timestamp of the command that the threat actor used to access a volume shadow copy of the beachhead host?

_Format: YYYY-MM-DD HH:MM:SS_

2024-03-05 21:23:17

You can use event id 4688 to find the volume shadow copy program that was executed.

<img width="1920" height="48" alt="Lab 1 22" src="https://github.com/user-attachments/assets/c786c3b4-ae15-43e1-8b55-bbb257fc42bc" />

-------------------------------------------------------------------------------
What was the file name that was responsible for running the volume shadow copy command?

<img width="815" height="84" alt="Lab 1 23" src="https://github.com/user-attachments/assets/5eb01089-e968-4c82-9bc8-94512e64a89b" />

2024-03-05 21:23:17

-------------------------------------------------------------------------------
## Looking Around The Network

The threat actor was able to open a file that was supposed to be in a hidden share from the beachhead host.

What was the name of the file?

_*Note: Just have one extension not two in the answer_

you-cant-see-this-cause-I-am-good-at-NTFS-permissions.txt.txt

<img width="1920" height="917" alt="Lab 1 30" src="https://github.com/user-attachments/assets/acc300cd-2eaa-42f9-b35f-0bf6faa918ae" />

-------------------------------------------------------------------------------
What discovery tool did the threat actor create through their beacon?

_Format: Mimikatz.exe_

<img width="1923" height="924" alt="Lab 1 13" src="https://github.com/user-attachments/assets/7a2c8b9e-f535-4339-be43-991efc0b414c" />


-------------------------------------------------------------------------------
Was the threat actor successful in executing this tool?

NO events for 4688.

-------------------------------------------------------------------------------
What group did the malicious process enumerate on the beachhead host?

_Format: Remote Desktop Users_

Administrators

<img width="478" height="29" alt="Lab 1 25" src="https://github.com/user-attachments/assets/e351ffe3-82fc-4df8-aab2-d66600068c38" />

-------------------------------------------------------------------------------
## Domain Dominance

What was the name of the malicious service that the threat actor attempted to install on the Domain Controller?

8628f7b

<img width="1921" height="909" alt="Lab 1 26" src="https://github.com/user-attachments/assets/ca42dd08-c5ad-47ca-a361-147f97f2773a" />

-------------------------------------------------------------------------------

What was the possible operating system distribution the threat actor is using?

parrot

<img width="1923" height="915" alt="Lab 1 27" src="https://github.com/user-attachments/assets/257366a3-a76e-4f66-ae0f-c4a5dabab45f" />


-------------------------------------------------------------------------------

What was the name of the DLL installed onto one of the domain controller's processes to monitor for credentials?


<img width="1921" height="914" alt="Lab 1 28" src="https://github.com/user-attachments/assets/f993ca9e-bcab-4f4c-bd1e-c7e5fab933d3" />


<img width="1922" height="916" alt="Lab 1 29" src="https://github.com/user-attachments/assets/1e2c2146-90a1-44cb-b6f5-06e909d157e0" />


Location of PTASpy.dll file.

-------------------------------------------------------------------------------

What was the file name of the process that this DLL was injected to?


AzureADConnectAuthenticationAgentService.exe

<img width="1921" height="914" alt="Lab 1 28 1" src="https://github.com/user-attachments/assets/afaa777a-56cc-413a-a615-22671d5572d1" />


-------------------------------------------------------------------------------

What is the likely user that had their credentials recorded by the DLL.

The user account whose credentials was most likely stolen was cpecht7@waifu.phd

-------------------------------------------------------------------------------
## Accessing the Good Stuff

Which account did the threat actor access the SQL server with via RDP?

cc-admin


<img width="1611" height="64" alt="Lab 1 31" src="https://github.com/user-attachments/assets/b38130c9-e05f-4cfc-98b8-95579424ca12" />

-------------------------------------------------------------------------------

The University admins noticed a strange file in the documents folder of the admin user for the SQL server which was created during the intrusion. What was the earliest MFT Entry ID for the file name in this path?

<img width="1407" height="49" alt="Lab 1 34" src="https://github.com/user-attachments/assets/09fb6426-a26e-46d4-a72b-9771c57aa338" />


-------------------------------------------------------------------------------
## Release the Ransomware UwU

What was the name of the ransomware binary?

print64.exe

<img width="1298" height="53" alt="Lab 1 35" src="https://github.com/user-attachments/assets/4ee3915a-4c34-4729-9bf5-3c71a541898b" />
<img width="1921" height="919" alt="Lab 1 36" src="https://github.com/user-attachments/assets/112475ab-ec9f-41de-bd3b-f1c247ac1cfa" />



What was the access token provided to the binary to decrypt the payload?

uwuwuwuwuwuwuw
<img width="1921" height="919" alt="Lab 1 36 1" src="https://github.com/user-attachments/assets/ab115af4-8269-4771-addf-4c4a68e10b09" />


