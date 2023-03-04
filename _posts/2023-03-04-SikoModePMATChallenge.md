---
layout: post
title: Siko Mode
---

## References
-PMAT LABS
-Siko Mode Challenge 2

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Challenge Questions:

What language is the binary written in?

- Nim

![image](https://user-images.githubusercontent.com/122228333/222918135-053318dd-4bf2-49e0-9bae-362bab061839.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What is the architecture of this binary?

- Architecture of binary 64 bit.

![image](https://user-images.githubusercontent.com/122228333/222917955-629e4de1-f88e-47ee-ad21-1412262ae580.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Under what conditions can you get the binary to delete itself?

- Will Delete itself if its not contacting out to Internet. This is done when you dont have INETSIM running the file will delete itself from disk.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Does the binary persist? If so, how?

- No does not persist. Does not write to disk.

![image](https://user-images.githubusercontent.com/122228333/222919560-03016316-27b7-41b7-8f04-bf84747895f2.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What is the first callback domain?

![image](https://user-images.githubusercontent.com/122228333/222920091-b313804b-7ac1-46ce-9b4c-61d825764736.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------


Under what conditions can you get the binary to exfiltrate data?

- The binary can exfiltrate data by contacting callback domain url.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What is the exfiltration domain?

- Wireshark Get Request. hxxp://cdn.altimiter.local

![image](https://user-images.githubusercontent.com/122228333/222921926-5669d355-920e-48eb-ae65-787ff9fd3b0b.png)


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

How does exfiltration take place?

- The exfiltration takes place by first checking to see if there is a internet connection. Internet connection you move on to the next step which is calling to the exfiltration domin to extract data from disk. 

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What URI is used to exfiltrate data?

- The uri that is used with the exfiltrate data is http://cdn.altimiter.local/feed?post=[data].

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What type of data is exfiltrated (the file is cosmo.jpeg, but how exactly is the file's data transmitted?)

- The file is encrypted with rc4 on cosmo.jpeg. The file cosmo.jpeg contents are encrypted by the password siko mode.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What kind of encryption algorithm is in use?

- RC4 Based on Strings also you can use cutter to look at where the call is for RC4.

![image](https://user-images.githubusercontent.com/122228333/222921797-0a21f5d9-66e7-4acb-bf6f-37360a87aa08.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What key is used to encrypt the data?

- Password.txt ( Siko Mod )

![image](https://user-images.githubusercontent.com/122228333/222922013-8dea78ff-1368-4a1e-9c5c-9fa17aa6170a.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

What is the significance of houdini?

- The significance of houdini is Deleting binary from disk. 
