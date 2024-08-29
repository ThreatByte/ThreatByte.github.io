---
layout: post
title: Practical Windows Forensic Results
---

## References
-PWF 
- https://github.com/bluecapesecurity/PWF

### System Info

- Computername: 
  DESKTOP-T3DDCIU
  
- Windows Version: 
  ProductName Windows 10 Enterprise Evaluation
  BuildLab                  19041.vb_release.191206-1406 
  ReleaseID                 2009
  CompositionEditionID      EnterpriseEval
  RegisteredOrganization
  RegisteredOwner           IEUser
  UBR                       2006
  InstallDate               2024-04-23 20:02:45Z
  InstallTime               2024-04-23 20:02:45Z
                                                                            

- Timezone:
  Pacific Standard Time

- Network Information: 
  DhcpIPAddress                10.0.2.15
  DhcpSubnetMask               255.255.255.0
  DhcpServer                   10.0.2.2
  DhcpNameServer               192.168.0.1
  DhcpDefaultGateway           10.0.2.2
  DhcpSubnetMaskOpt            255.255.255.0

- Shutdown time: 
    ShutdownTime  : 2024-04-23 17:39:18Z

- Defender settings:
  LastWrite Time: 2024-04-23 17:28:10Z
  DisableRealtimeMonitoring value = 1



### Users Groups and User Profiles

- Active accounts during the attack timeframe?
    Username        : IEUser [1001]
    SID: S-1-5-21-3806393029-790903772-3070350119-1001                                                                                                                                                                                                                                                     
    Acc Creation:  Tue Apr 23 20:03:58 2024 Z

- Which account(s) were created?
  Username        : art-test [1002]
  Account Created : Tue Apr 23 17:32:03 2024 Z
  
- Which accounts are Administrator group members?
  IEUSER and art-test
  
- Which users have profiles?
  Path      : C:\Users\IEUser
  SID       : S-1-5-21-3806393029-790903772-3070350119-1001
  LastWrite : 2024-04-23 17:39:12Z                                                                    


### User Behavior

- CEB - List of applications, files, links, and other objects that have been accessed 
    2024-04-23 17:27:55Z
    Microsoft.Windows.Explorer (12)


- F4E - List of shortcut links used to start programs
  {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\WindowsPowerShell\v1.0\powershell.exe
  {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\notepad.exe


- RecentDocs: 		Files and folders opened
    No Documents Open

- Shellbags:		Locations browsed by the user
    2024-04-23 17:27:49  My Computer\CLSID_Desktop\PWF-main [Desktop\1\3\0\]
    2024-04-23 17:28:23  PWF-main\PWF-main [Desktop\4\0\]
    2024-04-23 17:28:55  PWF-main\PWF-main\AtomicRedTeam [Desktop\4\0\1\]
      
  ### NTFS

- Which files are located in My Computer\CLSID_Desktop\PWF-main\PWF-main\AtomicRedTeam?
    ART-attack-cleanup.ps1
    ART-attack.ps1
    PWF_Analysis-MITRE.png
    PWF_Analysis-MITRE.svg

- What is the MFT Entry Number for the file "ART-attack.ps1"?
  SI:
    Created On:         2024-04-23 17:27:49.1785014
    Modified On:        2024-04-23 17:27:49.1785014
    Record Modified On: 2024-04-23 17:27:49.1785014
    Last Accessed On:   2024-04-23 17:27:49.1785014
  FN:
    Created On:         2024-04-23 17:27:49.1785014
    Modified On:        2024-04-23 17:27:49.1785014
    Record Modified On: 2024-04-23 17:27:49.1785014
    Last Accessed On:   2024-04-23 17:27:49.1785014
    
- What are the MACB timestamps for "ART-attack.ps1"?
  Modified		m... 	2024-04-23 17:27:49.1785014
  Accessed		.a.. 	2024-04-23 17:27:49.1785014
  Changed ($MFT)	..c.	2024-04-23 17:27:49.1785014
  Birth (Creation)	...b	2024-04-23 17:27:49.1785014

- Was "ART-attack.ps1" timestomped?
  Yes

- When was the file "deleteme_T1551.004" created and deleted?
  2024-04-23 17:34:28 - Created
  2024-04-23 17:34:35 - Deleted


- What was the Entry number for "deleteme_T1551.004" and does it still exist in the MFT?
  108124 entry number
  overwritten


### Execution Artifacts

- Which executables (.exe files) did the BAM record for the IEUser (RID 1001) incl. their last execution date and time? 
  2024-04-23 17:39:11Z - \Device\HarddiskVolume2\Windows\explorer.exe
  2024-04-23 17:39:11Z - \Device\HarddiskVolume2\Windows\System32\ApplicationFrameHost.exe
  2024-04-23 17:39:11Z - \Device\HarddiskVolume2\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
  2024-04-23 17:32:19Z - \Device\HarddiskVolume2\Windows\System32\cmd.exe
  2024-04-23 17:39:11Z - \Device\HarddiskVolume2\Windows\System32\notepad.exe


- Determine the cache entry position for: 
  ⦁	AtomicService.exe: 8 
  ⦁	mavinject.exe: 9

### Persistence Mechanisms

- What is the full path of the AtomicService.exe that was added to the run keys?

  C:\Path\AtomicRedTeam.exe

- What is the name of the suspicious script in the StartUp folder?

  batstartup.bat

- When was the suspicious atomic service installed?

  2024-04-23 17:32:19Z


- Which tasks were created by the IEUser and what's the creation time? 
  
  T1053_005_OnLogon
  LastWrite: 2024-04-23 17:32:16Z
  Id: {53DBC56D-9B8C-4E09-A929-E2ED2A7A9176}
  Task Reg Time: 2024-04-23 17:32:16Z
  
  T1053_005_OnStartup
  LastWrite: 2024-04-23 17:32:16Z
  Id: {75E6707F-2AE8-4E27-9E33-39D9F453BD45}
  Task Reg Time: 2024-04-23 17:32:16Z



- How many times did they execute?
  Never
  
### Memory Analysis 

- PID of suspicious processes?
  powershell.exe	<6978>
  notepad.exe		<6553>
  AtomicService.exe	<6560>

- Suspicious registry key in HKCU?
  iex ([Text.Encoding]::ASCII.GetString([Convert]::FromBase64String((gp "HKCU:\Software\Classes\AtomicRedTeam').ART
 

