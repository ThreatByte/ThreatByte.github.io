---
layout: post
title: Xintra KG Distribution Writeup
---

## References
[https://www.xintra.org/labs](https://www.xintra.org/labs)

https://www.youtube.com/@13Cubed

https://attack.mitre.org/tactics/enterprise/

https://github.com/mandiant/SharPersist

[https://www.ninjaone.com/it-hub/endpoint-management/what-is-a-windows-minidump-file/](https://www.ninjaone.com/it-hub/endpoint-management/what-is-a-windows-minidump-file/)

## Scoping
Last week, Patricia Bethel started her new job at KG Distribution. Amid the rush of HR and other onboarding tasks, Patricia received an email from IT Support urging her to complete an important task. Trusting the source, Patricia carefully and diligently followed the instructions before continuing with her workday.

Several hours later, the Security Operations Center (SOC) helpdesk received an alert regarding unusual behavior on one of the domain controllers. As part of their incident response protocol, the SOC remotely captured volatile evidence from this system, as well as from any other systems that had recently interacted with it in an atypical manner. Since it is now after normal business hours, the SOC has not yet been able to reach Patricia to question her about any activity on her system or to analyze the system itself. KG Distribution is also reluctant to take the domain controller offline and risk an outage, even after hours, until they are certain that the activity is indeed malicious.

As the Senior Security Engineer on call for after-hours alerts, it is now your responsibility to analyze the available evidence. Your task is to determine whether a security incident has occurred, and if so, identify the key details of the incident to decide if further action is necessary.

## Analysis Summary Workstation

A malicious file called File Action Required IT System Upgrade.eml was opened which downloaded a file called dwagent.exe. The threat actor then used a file transfer tool called curl.exe to download a malicious binary called OfficeUPgrade.exe which was executed roughly one minute after curl.exe was created. OfficeUPgrade.exe survived reboots by maintaining persistence by editing the run key. Also, a file was created called temp.ps1.

## Initial Access Workstation

The user opened the phishing email Action Required IT System Upgrade.eml on 2024-08-18 16:31:58 UTC. 

![Part1-WORKSTATION - KG Distribution - XINTRA - Brave](https://github.com/user-attachments/assets/0d92e584-edcb-40e3-8ee9-76f07eed10e7)



Remote administration tool dwgagent.exe was downloaded to gain a foothold on the system after File Action Required IT System Upgrade.eml was opened this occured on 2024-08-18 16:33:31 UTC. The IP address that the attacker used to download OfficeUPgrade.exe was 64.23.144.215 on port 8888 this IP address was flagged by virus total as malicious.

![Part2](https://github.com/user-attachments/assets/1b766c4b-c43b-4304-bc27-f209b46ff0dd)


![Part 6](https://github.com/user-attachments/assets/c0b70b71-0201-42de-9eec-16d2a1736ca5)



## Execution Workstation

The earliest execution of the remote administration dwagent.exe running on the system was 2024-08-18 16:33:30 UTC. Threat actor executed curl.exe to download a malicious binary this occurred at 2024-08-18 16:53:46 UTC. OfficeUPgrade.exe was then executed approximately one minute after curl.exe was executed at 2024-08-18 16:54:46 UTC. Malicious script Temp.ps1 was dropped on system at 2024-08-18 17:36:01 once user executed OfficeUPgrade.exe. OfficeUPgrade.exe malicious dll was COFFLoader.x64.dll. Powershell script temp.ps1 malicious dll is CMSTP-UAC-Bypass.dll.

![Part 3](https://github.com/user-attachments/assets/67dcb316-5e4c-4559-99a6-8b1da6dd2712)

![Part 4](https://github.com/user-attachments/assets/f891b1a7-86e5-44cd-bebc-4580edd0029a)

![Part 5](https://github.com/user-attachments/assets/0ebc6b54-a55d-4780-96f2-1c3c5e398287)

![Part 8](https://github.com/user-attachments/assets/42c7be5b-f5c4-4a53-bc72-1b5baba027f9)

![Part 9](https://github.com/user-attachments/assets/e89c06d6-fbc6-4cc4-84d9-38104fe093ea)

![Part 10](https://github.com/user-attachments/assets/2c285d43-5866-44e6-a9ba-87bcc4cb4d6c)

![Part 11](https://github.com/user-attachments/assets/65a7d9bd-828c-45f1-951c-106c7a70932e)


## Persistence Workstation

OfficeUPgrade.exe maintained persistence on the workstation by editing the registry key HKLM\Software\Microsoft\Windows\CurrentVersion\Run.

![Part 7](https://github.com/user-attachments/assets/64ba8c15-2220-45d1-8b22-91132de2d0f3)


## Analysis Summary Server

A malicious file called rGARTERny.exe was found on the server. The location of the file was in the temp directory. Service was created called officeupgradeservice. A encoding occurred on power shell process which was the child process of rGARTERny.exe. rGARTERny.exe persisted on the system by targeting the startup folder. The threat actor attempted to use a tool called SharpPersist to stay undetected on system. Threat actor used command and control server silver to connect to system and ex filtrate data by targeting ntds.dit, and SYSTEM.

## Execution

rGARTERny.exe when executed parent process was service.exe. Threat actor created a new service called officeupgradeservice. From the parent process rGARTERny.exe a child process was spawned called powershell the command was encoded using UTF8. 

![Serv 1](https://github.com/user-attachments/assets/15c56877-0b0a-4e01-a34c-4dbd17baf651)

![Serv 1 1](https://github.com/user-attachments/assets/43841c6a-2863-47b1-81ba-193ff32182d8)

![Serv 1 2](https://github.com/user-attachments/assets/0e54be8f-4761-4148-bf31-ded79eef9bdf)

![Serv 2](https://github.com/user-attachments/assets/79ae5aa1-c34a-421f-aa96-9af7da7e7aed)

![Serv 2 1](https://github.com/user-attachments/assets/b8e75c67-503c-40de-bb47-764266e301f3)

![Serv 2 2](https://github.com/user-attachments/assets/95191093-b3ce-4a8a-ad08-f3a4cf608103)

![Serv 2 3](https://github.com/user-attachments/assets/9d2126be-379c-4f24-aaad-1c741c1930a8)



## Persistence

The Threat actor maintained persistence by targeting the Startup folder. A malicious binary called OfficeUpgrade was found in the startup folder. The threat actor attempted to use a persistence tool called SharpPersist which is a red team tool. Finding SharpPersist was difficult had to look in the minidump.dmp file which is a file that is created when a workstation or server runs into a error that is caused by Windows Blue Screen of Death.

SharpPersist - https://github.com/mandiant/SharPersist
minidump.dmp - https://www.ninjaone.com/it-hub/endpoint-management/what-is-a-windows-minidump-file/

![Serv 2 4](https://github.com/user-attachments/assets/854dd6dd-6a3f-46dc-bd02-342dba8b2936)

![Serv 2 5](https://github.com/user-attachments/assets/445e989d-cd4d-44bd-89dc-d0d6a6c3fbfd)

![Serv 2 6](https://github.com/user-attachments/assets/4297888f-8913-4c44-be4f-8a0116782aaf)



## Command and Control

rGARTERny.exe was flagged by Yara rule that stated rGARTERny.exe is a command and control server called Silver. rGARTERny.exe connected to the server from a remote ip address 64.23.144.215 and port number 8888. 

![Serv 2 7](https://github.com/user-attachments/assets/cdf67dc9-1219-486c-8e7b-f5841b4e7c3c)


## Exfiltration

Threat actor attempted to steal ntds.dit and SYSTEM files from the server. The ntds.dit file contains active directory information about users and password hashes. ntds.dit exfil occured on 2024-08-18 18:04:56 UTC and SYSTEM exfil occured 2024-08-18 18:06:07 UTC.

ntds.dit - https://blog.netwrix.com/2021/11/30/extracting-password-hashes-from-the-ntds-dit-file/


![Serv 2 8](https://github.com/user-attachments/assets/d1c66300-f113-4e55-b3fd-25a6e7097dbc)



