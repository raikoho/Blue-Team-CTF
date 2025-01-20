## Artefacts

![image](https://github.com/user-attachments/assets/115d5828-15ed-4537-a133-ca5bf453db9f)

The email appeared questionable, but Evan still opened the attachment despite the scepticism. After opening the attached document and seeing that nothing happened, Evan reported the phishing email to the security team.

Upon receiving the phishing email report, the security team investigated the workstation of the CEO. During this activity, the team discovered the email attachment in the downloads folder of the victim.

![image](https://github.com/user-attachments/assets/55c194f4-29da-4824-abf5-fcfdd7321017)

In addition, the security team also observed a file inside the ISO payload, as shown in the image below.

![image](https://github.com/user-attachments/assets/ef383625-ab11-482b-9238-31e27d08f1e4)

Lastly, it was presumed by the security team that the incident occurred between August 29 and August 30, 2023.

Given the initial findings, you are tasked to analyse and assess the impact of the compromise.

## Start

### What is the PID of the process that executed the initial stage 1 payload?

![image](https://github.com/user-attachments/assets/6d77e7c4-7010-40f6-a540-3d1427a538d5)

![image](https://github.com/user-attachments/assets/50c899c7-6064-4c11-85fc-215aa6f4e944)


### The stage 1 payload attempted to implant a file to another location. What is the full command-line value of this execution?

![image](https://github.com/user-attachments/assets/22319ac0-4a38-49db-ac23-88325657434b)

![image](https://github.com/user-attachments/assets/22ba49a4-3c7f-4a6d-bdbf-97deb1915d4e)

we can see from the image a file is being copied using:

``"C:\Windows\System32\xcopy.exe" /s /i /e /h D:\review.dat C:\Users\EVAN~1.HUT\AppData\Local\Temp\review.dat``

### The implanted file was eventually used and executed by the stage 1 payload. What is the full command-line value of this execution?

![image](https://github.com/user-attachments/assets/b996c0df-4f68-48dc-85e4-5fc0b3b48aed)

### The stage 1 payload established a persistence mechanism. What is the name of the scheduled task created by the malicious script?

![image](https://github.com/user-attachments/assets/c2363849-8a3d-4cb4-84e9-dc0587ebe1af)

the same image. just look above. The same structure.

Answer: Review

### The execution of the implanted file inside the machine has initiated a potential C2 connection. What is the IP and port used by this connection? (format: IP:port)

go filter by event 3 :

![image](https://github.com/user-attachments/assets/095f1348-918f-4622-96b9-b2ae4173acc4)

add destination ip filter.

![image](https://github.com/user-attachments/assets/6a3593b9-b3f0-498f-a94e-c998052022c4)

### The attacker has discovered that the current access is a local administrator. What is the name of the process used by the attacker to execute a UAC bypass?

![image](https://github.com/user-attachments/assets/dcb86e15-f431-43f0-bdd8-8459d272ba20)

fodhelper.exe

### Having a high privilege machine access, the attacker attempted to dump the credentials inside the machine. What is the GitHub link used by the attacker to download a tool for credential dumping?

filter : *github*
and we can see mimikatz.

![image](https://github.com/user-attachments/assets/17909601-4653-4706-8465-14e9986c9174)

### After successfully dumping the credentials inside the machine, the attacker used the credentials to gain access to another machine. What is the username and hash of the new credential pair? (format: username:hash)

search for mimikatz and also set event id to 1

![image](https://github.com/user-attachments/assets/35643857-60e9-49d8-be21-f917eee06414)

### Using the new credentials, the attacker attempted to enumerate accessible file shares. What is the name of the file accessed by the attacker from a remote share?

Scroll downin the previous image. 

![image](https://github.com/user-attachments/assets/a61b5615-3591-4136-9dac-44a3c6de7bdd)

Answer: IT_Automation.ps1

### After getting the contents of the remote file, the attacker used the new credentials to move laterally. What is the new set of credentials discovered by the attacker? (format: username:password)

QUICKLOGISTICS\allan.smith:Tr!ckyP@ssw0rd987

![image](https://github.com/user-attachments/assets/3e850a9e-5ea3-4b8e-8c7d-0c3f4b2eee94)

### What is the hostname of the attacker's target machine for its lateral movement attempt?

Same picture.

![image](https://github.com/user-attachments/assets/e7360b33-5a26-471d-a296-95db1f18f08f)

### Using the malicious command executed by the attacker from the first machine to move laterally, what is the parent process name of the malicious command executed on the second compromised machine?

Filter events with Event ID of 1 & WKSTN-1327”

![image](https://github.com/user-attachments/assets/06686fce-8a81-4dd4-95af-b889921138a5)

### The attacker then dumped the hashes in this second machine. What is the username and hash of the newly dumped credentials? (format: username:hash)

administrator:00f80f2538dcb54e7adc715c0e7091ec

refer above image

### After gaining access to the domain controller, the attacker attempted to dump the hashes via a DCSync attack. Aside from the administrator account, what account did the attacker dump?

Change host filter to DC01:

![image](https://github.com/user-attachments/assets/97fb721a-c415-41e3-a21d-0181ce71ea5a)

Answer: backupda

### After dumping the hashes, the attacker attempted to download another remote file to execute ransomware. What is the link used by the attacker to download the ransomware binary?

Use the same all as previous section, just scroll down a little or use .exe filter in search:

![image](https://github.com/user-attachments/assets/77c87ace-a132-4ee2-9f0b-72caa5dafa7a)

