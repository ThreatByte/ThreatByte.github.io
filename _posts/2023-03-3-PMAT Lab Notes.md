---
layout: post
title: Malware Analysis PMAT Labs Notes
---


![NetworkDiagram](https://user-images.githubusercontent.com/122228333/222871356-1d63ac03-66a4-4e2d-8f26-c4fd624430c4.png)

-------------------------------------------------------------------------------------------------------------------------

## INETSIM
- INETSIM Fake Internet. 
- Routes traffic to where inetsim is set up.
- Remove Comment start_service dns
- service_bind_address 0.0.0.0
- dns_default_ip 192.168.0.4

-------------------------------------------------------------------------------------------------------------------------

## Basic Static
- Hash File
- Threat Intelligence Virus Total
- Strings(Floss, Strings)
- PE View(Analyze Portable Executables), pestudio(updated)
- CAPA(Detect Malicious Functions Program)

------------------------------------------------------------------------------------------------------------------------

## PE
- Image_File_Header Compile Time Time Date Stamp
- Virtual Size(Amount of Data on Disk Binary Run) & Size Raw Data
- Import Address Table (Win API)

------------------------------------------------------------------------------------------------------------------------
## Dynamic
- Run File
- Host(Does Something Workstation) / Network(Domain)
- C:\Windows\System32\drivers\etc

------------------------------------------------------------------------------------------------------------------------
## Advanced Static & Dynamic
- Static:
  - Decompilers & Dissassemblers(Cutter Look at code cant run, but can edit)
  - ASM(Instructions)(Human-readable)
- Dynamic:
  - Debuggers
    - 32dbg/64dbg
    - F9 Run
    - F8 Move Down Step over
    - F7 Step into
- CPU Instructions:
  - mov edx, eax ( mov eax into edx )
  - Stack Grows Down ( Push Values on top, Pop Bottom)
  - Eax, Ebx, Ecx, Edx, Ebp(Base Pointer), Eip(Instruction Pointer), Esp(Stack Pointer)
  - Last In First Out(Stack)
 

