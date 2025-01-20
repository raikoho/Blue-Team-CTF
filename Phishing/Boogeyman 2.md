## Artefacts

For the investigation, you will be provided with the following artefacts:

Copy of the phishing email.
Memory dump of the victim's workstation.
You may find these files in the /home/ubuntu/Desktop/Artefacts directory.

## Tools

- Volatility - an open-source framework for extracting digital artefacts from volatile memory (RAM) samples.
  ![Скриншот 20-01-2025 13 22 58](https://github.com/user-attachments/assets/827b92b5-23fe-4e6f-8f20-7cdf41241814)

- Olevba - a tool for analysing and extracting VBA macros from Microsoft Office documents. This tool is also a part of the Oletools suite.
![Скриншот 20-01-2025 13 23 18](https://github.com/user-attachments/assets/b6db5d7a-efcc-4d11-ad01-a757b3a60dc9)

## Start

Maxine, a Human Resource Specialist working for Quick Logistics LLC, received an application from one of the open positions in the company. Unbeknownst to her, the attached resume was malicious and compromised her workstation.

![image](https://github.com/user-attachments/assets/8c20ce29-4193-4674-b00f-9c9ba27d5d16)

SAVE the document attached from the email. GRAB its HASH - CHECK this HASH on the VirusTOTAL - it is malicious and can give some info!!

![Скриншот 20-01-2025 13 28 46](https://github.com/user-attachments/assets/1bc08e15-b0ce-4ecd-8792-b8e0797fc3e3)

### What email was used to send the phishing email?
westaylor23@outlook.com


### What is the email of the victim employee?
maxine.beck@quicklogisticsorg.onmicrosoft.com


### What is the name of the attached malicious document?
Resume_WesleyTaylor.doc

### What is the MD5 hash of the malicious attachment?

![Скриншот 20-01-2025 13 31 51](https://github.com/user-attachments/assets/2e00683a-48fe-435b-a99f-5e168d1b495d)

### What URL is used to download the stage 2 payload based on the document's macro?

i used:

``strings Resume_WesleyTaylor.doc`` 

![Скриншот 20-01-2025 13 41 22](https://github.com/user-attachments/assets/0404cd95-6d07-4e8f-a9c8-fbd1a1e58bd0)

Answer: https://files.boogeymanisback.lol/aa2a9c53cbb80416d3b47d85538d9971/update.png

or USE:

``olevba Resume_WesleyTaylor.doc``

![Скриншот 20-01-2025 13 44 24](https://github.com/user-attachments/assets/1e80fa98-8d28-4532-bb16-80e453188ea8)

### What is the name of the process that executed the newly downloaded stage 2 payload?

![Скриншот 20-01-2025 13 46 14](https://github.com/user-attachments/assets/f560b899-d78a-4164-a70b-eac69c8d3750)

wscript.exe

### What is the full file path of the malicious stage 2 payload?

![Скриншот 20-01-2025 13 47 16](https://github.com/user-attachments/assets/529ec0ad-fa2e-4160-954f-de5bcd97d17c)

C:\ProgramData\update.js

### What is the PID of the process that executed the stage 2 payload?

![Скриншот 20-01-2025 13 50 49](https://github.com/user-attachments/assets/88919b35-25de-464d-aa33-d7133e0b3752)

![image](https://github.com/user-attachments/assets/f6bedfcc-2cfc-4db4-bf0d-a940e758309b)

So use this command and grep `wscript` to see the PID of process.

### What is the parent PID of the process that executed the stage 2 payload?

Answer - 1124. Se previous output, but dont `grep` this time to see more.

### What URL is used to download the malicious binary executed by the stage 2 payload?

``strings WKSTN-2961.raw | grep boogeymanisback.lol``

![image](https://github.com/user-attachments/assets/de33b407-0ff5-49e2-8871-a43697f37b17)

### What is the PID of the malicious process used to establish the C2 connection?

``vol -f WKSTN-2961.raw windows.netscan``

![image](https://github.com/user-attachments/assets/23286ada-7f49-4840-ae01-3ed855934336)

### What is the full file path of the malicious process used to establish the C2 connection?

``vol -f WKSTN-2961.raw windows.dlllist --pid 6216``

![image](https://github.com/user-attachments/assets/aadae691-a831-42ab-a019-3b9c7cb009f0)

### What is the IP address and port of the C2 connection initiated by the malicious binary? (Format: IP address:port)

``vol -f WKSTN-2961.raw windows.netscan``

look for the "update.exe"
![image](https://github.com/user-attachments/assets/c9da72aa-e32e-46ff-8f6d-a80e13d51f2e)

### What is the full file path of the malicious email attachment based on the memory dump?

``vol -f WKSTN-2961.raw windows.filescan | grep Resume_WesleyTaylor`` show all files modifies/opens

![image](https://github.com/user-attachments/assets/c2961a53-3d6a-4571-8e51-8e0812eae1ca)

### The attacker implanted a scheduled task right after establishing the c2 callback. What is the full command used by the attacker to maintain persistent access?

``strings WKSTN-2961.raw | grep schtasks``

![image](https://github.com/user-attachments/assets/ec6334a7-0a24-41ac-8cb0-3d2a266ca2af)
