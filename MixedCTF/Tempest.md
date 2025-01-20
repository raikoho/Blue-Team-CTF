## Intro and Objectives

We have this Incedent Files:

![Скриншот 20-01-2025 11 37 25](https://github.com/user-attachments/assets/c15f57ec-79e9-4a9a-a406-2d2d5fd18c89)

Now we can follow the questions below
-------------------

## GET HASH

![Скриншот 20-01-2025 11 38 00](https://github.com/user-attachments/assets/2b4d94cb-40a7-4f28-aeb3-2515d49b6b0d)

``> Get-FileHash -Algorithm SHA256 .\capture.pcapng``


To parse the provided logs, we need first to convert the EVTX logs into CSV using EvtxEcmd and then feed it into Timeline Explorer.

![Скриншот 20-01-2025 11 42 38](https://github.com/user-attachments/assets/01f6160e-3237-441a-b788-60c5243a75e7)

## Know what exactly file was downloaded

Use Explorer Timeline to view the Incedent Files. We have info that the file was downloaded using chrome.exe. filter for event id 11 (file creation) and use find to search .doc you’ll get the answer in payload data4 towards the right. So Search for this keywords:
Answer: free_magicules.doc
![Скриншот 20-01-2025 12 07 36](https://github.com/user-attachments/assets/ffaaef3b-fdb6-4093-a935-bafdb6477d1e)

## Know Username and Machine

Anser: benimaru-TEMPEST

you’ll find it username column on the previous screenshot

## What is the PID of the Microsoft Word process that opened the malicious document? And Program Name?

Find it using "free_magicules.doc" keywords in the search:

![Скриншот 20-01-2025 12 16 27](https://github.com/user-attachments/assets/b81242d8-1482-433d-bd14-4b6f070c4176)

![Скриншот 20-01-2025 12 17 18](https://github.com/user-attachments/assets/c7d7eb0d-8d3d-4025-8745-77b5209e3d86)

## Based on Sysmon logs, what is the IPv4 address resolved by the malicious domain used in the previous question?

Ans: 167.71.199.191

keeping the winword.exe in the search and filter event id 22 for dns ,we can see the resolved ip address in payload data 6

![image](https://github.com/user-attachments/assets/1202a0d6-6aa5-4bf5-bf34-8be5c9cdec4c)

## What is the base64 encoded string in the malicious payload executed by the document?

setting up parentprocess id to 496 to column payload data 4 and set event id 1 for process creation ,check the executable info

![Скриншот 20-01-2025 12 21 13](https://github.com/user-attachments/assets/cdd790fd-e0dc-4e90-a26b-4fcf75b163a2)

## What is the CVE number of the exploit used by the attacker to achieve a remote code execution?

Answer: 2022–30190

msdt.exe is the process that executed the malicious base64 payload. search for msdt.exe cve

## The malicious execution of the payload wrote a file on the system. What is the full target path of the payload?

![image](https://github.com/user-attachments/assets/a636bf55-d664-4068-836b-e63a4f7d9f3f)

Answer: C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

from the decoded payload we can see a file update.zip is being downloaded and placed in startup folder, search for the file name and get the full path in payload data4.

## The stage 2 payload downloaded establishes a connection to a c2 server. What is the domain and port used by the attacker?

Ans: resolvecyber.xyz:80

you can use the sysmonview tool search select first.exe and select image path then value under sessions, this will give you a good overview

![image](https://github.com/user-attachments/assets/42efa8bd-13ce-4dfa-94c5-c472abd66e0d)





