---
layout: post
title: Wanna Cry
---

## References
- PMAT LABS
- Wanna Cry

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Challenge Questions

Record any observed symptoms of infection from initial detonation. What are the main symptoms of a WannaCry infection?

- Main systems of the wannacry malware is encrypting your files, window shows up timer, bitcoin address, timer.

![image](https://user-images.githubusercontent.com/122228333/222925951-efd8c950-cd57-478a-bead-c0cb6ee98fb2.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Use FLOSS and extract the strings from the main WannaCry binary. Are there any strings of interest?

![image](https://user-images.githubusercontent.com/122228333/222926333-73dccd71-ae9a-44e7-9e9d-779a9759cebf.png)

![image](https://user-images.githubusercontent.com/122228333/222926452-e398c046-8238-40d4-829d-eba061bb4c70.png)

![image](https://user-images.githubusercontent.com/122228333/222926501-1b7af183-744c-451f-96e1-d55d25eb1129.png)

- First Screenshot shows URL.
- Second Screenshot shows Shares that Wannacry is trying to contact.
- Third Screenshot shows different languages when user is infected the Ransomware message will change to that specific language.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

CAPA(MITRE ATT&CK FRAMEWORK)

![image](https://user-images.githubusercontent.com/122228333/222926699-2156310e-bb06-4d53-9128-222b2438e20a.png)

- CAPA Specifies what techniques that WannaCry uses maps MITRE ATT&CK FRAMEWORK.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Inspect the import address table for the main WannaCry binary. Are there any notable API imports?

![image](https://user-images.githubusercontent.com/122228333/222926773-0bb52a50-168a-40eb-834d-c6d7eddc15a4.png)

- Crypto imports will encrypt your workstation that you are currently using.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What conditions are necessary to get this sample to detonate?

- The Conditions are that if Wannacry sees that there is a internet connection. It will close the application and delete itself from disk.
- You can't run INET SIM to fake a internet connection.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------


Network Indicators: Identify the network indicators of this malware

![image](https://user-images.githubusercontent.com/122228333/222927028-6ed9ab04-79c2-48bc-8a2e-0332af9e148a.png)

- Scans a List of IP Addresses ( WORM ).

![image](https://user-images.githubusercontent.com/122228333/222927519-f31183d8-758b-4590-92a9-71fb8d01de77.png)


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Host-based Indicators: Identify the host-based indicators of this malware.

![image](https://user-images.githubusercontent.com/122228333/222927719-2442ab05-8833-4fb4-99d4-a5756799391f.png)

![image](https://user-images.githubusercontent.com/122228333/222927822-efe3bc81-04dc-46a6-9282-1ba3f83c6c78.png)

![image](https://user-images.githubusercontent.com/122228333/222928023-3086fe24-55a1-4a58-85ca-68f6806fc50f.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Use Cutter to locate the killswitch mechanism in the decompiled code and explain how it functions.

- Used DBG So i can change the instructions when the program is actually running.
- Find the URL Set Breakpoint ![image](https://user-images.githubusercontent.com/122228333/222928683-53ab22b0-ca30-4730-a422-7c391f6f54c8.png)
- Change Test edx edx zf to 1

