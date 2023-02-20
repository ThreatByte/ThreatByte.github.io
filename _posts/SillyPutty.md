---
layout: post
title: Silly Putty
---

## References
-PMAT LABS
-Silly Putty

## Challenge Questions:

### Basic Static Analysis
---

- What is the SHA256 hash of the sample?

- Run sha256.exe putty.exe 0c82e654c09c8fd9fdf4899718efa37670974c9eec5a8fc18a167f93cea6ee83

![image](https://user-images.githubusercontent.com/122228333/219960702-d8359e1a-d7c2-4f53-9a97-8a421dff34b8.png)

---------------------------------------------------------------------------------------------------------------

- What architecture is this binary?

- 32 bit

![image](https://user-images.githubusercontent.com/122228333/219960730-a5af69c8-562b-4119-a888-2384ae1b5d28.png)

----------------------------------------------------------------------------------------------------------------

- Are there any results from submitting the SHA256 hash to VirusTotal?

![image](https://user-images.githubusercontent.com/122228333/219960778-3376d89f-648c-4109-95a4-6f3c6a3bc550.png)

---------------------------------------------------------------------------------------------------------------

- Describe the results of pulling the strings from this binary. Record and describe any strings that are potentially interesting. Can any interesting information be extracted from the strings?

- One Important String Powershell. Looks like generic putty executable.

![image](https://user-images.githubusercontent.com/122228333/220145420-3e14946b-0e34-49ba-83be-6a1f635c9ca2.png)


---------------------------------------------------------------------------------------------------------------

- Describe the results of inspecting the IAT for this binary. Are there any imports worth noting?

-Some indicators that are marked as suspicious but looks like regular putty. 

![image](https://user-images.githubusercontent.com/122228333/219961591-e5afa8ad-270d-478e-ad2e-07c235b2ac7e.png)

---------------------------------------------------------------------------------------------------------------

- Is it likely that this binary is packed?

- Not Packet Virtual Size and Size of Raw Data Roughly the same.

- ![image](https://user-images.githubusercontent.com/122228333/219961838-35013b40-4eef-4b20-999d-e48292f2a885.png)

---------------------------------------------------------------------------------------------------------------



---

### Basic Dynamic Analysis
 - Describe initial detonation. Are there any notable occurrences at first detonation? Without internet simulation? With internet simulation?
 
 - Without Internet Simulation Putty comes up followed by a powershell prompted. With Internet Simulation Putty comes up followed by a powershell prompt.
 
 --------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
 - From the host-based indicators perspective, what is the main payload that is initiated at detonation? What tool can you use to identify this?
 
 - procmon filter process putty.exe 
 
 ![image](https://user-images.githubusercontent.com/122228333/220147330-7528cb06-55db-4afd-b1ea-4b6d50bf79fe.png)

 - filter parent id 
 
 ![image](https://user-images.githubusercontent.com/122228333/220147876-b7ba9138-2c1e-4424-afd0-5f4f0550d287.png)
 
 - Based on Information the string is encoded with base64
 
 - I did not know that you redirect payload to out file and extract file to find the code written in powershell. Learning something new 
 
 ![image](https://user-images.githubusercontent.com/122228333/220149863-efa8c0bc-d9d5-40bb-ac02-7f7bf28fbb64.png)

 - What is the DNS record that is queried at detonation?
 
 - 
 
 
 - What is the callback port number at detonation?
 - What is the callback protocol at detonation?
 - How can you use host-based telemetry to identify the DNS record, port, and protocol?
 - Attempt to get the binary to initiate a shell on the localhost. Does a shell spawn? What is needed for a shell to spawn?






