---
layout: post
title: Silly Putty
---

## References
-PMAT LABS
-Silly Putty Challenge 1

## Challenge Questions:

### Basic Static Analysis
---

- What is the SHA256 hash of the sample?

- 0c82e654c09c8fd9fdf4899718efa37670974c9eec5a8fc18a167f93cea6ee8

- What architecture is this binary?

- ![image](https://user-images.githubusercontent.com/122228333/222881522-d0bf8625-9015-40f9-8288-173f38793097.png)


- Are there any results from submitting the SHA256 hash to VirusTotal?

- Yes. Some of the results are that the file is part of PMAT Labs due to the amount of users submitting hash.

- ![image](https://user-images.githubusercontent.com/122228333/222881706-56b6b330-b60f-48fd-a492-df74f93fae5b.png)



- Describe the results of pulling the strings from this binary. Record and describe any strings that are potentially interesting. Can any interesting information be extracted from the strings?

- From Floss found Powershell Script.

- ![image](https://user-images.githubusercontent.com/122228333/222881905-f5d42b85-4f00-4341-ba01-d11ec61402d1.png)


- Describe the results of inspecting the IAT for this binary. Are there any imports worth noting?

- Nothing really that intresting looks like regular putty.exe


- Is it likely that this binary is packed?

- Not packet Size of Raw Data, Virtual Size Roughly Same

- ![image](https://user-images.githubusercontent.com/122228333/222882106-71e8c8a4-582d-49dc-88d2-2fdde518aeb1.png)




---

### Basic Dynamic Analysis
 - Describe initial detonation. Are there any notable occurrences at first detonation? Without internet simulation? With internet simulation?
 

 - From the host-based indicators perspective, what is the main payload that is initiated at detonation? What tool can you use to identify this?
 

 
 - Based on Information the string is encoded with base64
 
 
 - What is the DNS record that is queried at detonation?
 


 - What is the callback port number at detonation?

 
 - What is the callback protocol at detonation?
 
 
 
 - How can you use host-based telemetry to identify the DNS record, port, and protocol?
 

 
 - Attempt to get the binary to initiate a shell on the localhost. Does a shell spawn? What is needed for a shell to spawn?
 







