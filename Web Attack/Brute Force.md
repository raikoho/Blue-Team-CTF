Web server has been compromised.

Goal: investigate the breach.

File Location: /root/Desktop/File.pcap

## Walkthough Questions

### What is the IP address of the server targeted by the attacker's brute-force attack?

![image](https://github.com/user-attachments/assets/c983586f-49e1-4a44-9dd0-dab655a1b3e2)

i used filter `http` and have noticed that there is POST request to the index.php page.
I am sure, that it is login page, because this page was attacked very often, as if it was a brute force attack.

Answer: 51.116.96.181

### Which directory was targeted by the attacker's brute-force attempt?

From the screen above..

Answer: index.php

### Identify the correct username and password combination used for login.

![image](https://github.com/user-attachments/assets/f52cc4b9-179f-42bc-8b3e-d773ae8e0ef6)

![image](https://github.com/user-attachments/assets/df19729c-92f7-4277-91d7-a7a0a0f3c435)

Although the page status was always 200, I tried to find the difference in the length of the response in the request. The difference was only 1 character. I expanded the package further and:

Answer: web-hacker:admin12345

### How many user accounts did the attacker attempt to compromise via RDP brute-force?

RDP has this filter for Wireshark:

![image](https://github.com/user-attachments/assets/a81fd305-2efc-4156-b360-f2346a966685)

I can manually inspect TCP streams for login attempts. But
Since RDP authentication involves NTLM (Network Authentication) or CredSSP (Credential Security Support Provider Protocol), i looked for these authentication packets:

`rdp` or `rdp.security`

### What is the “clientName” of the attacker's machine?

![image](https://github.com/user-attachments/assets/510f4f1c-32b7-4ba1-937d-7f9cb9d36040)

preusedfilter: `tcp.port == 3389`
then I found it by using cntrl+f and find "client". 

Answer: t3m0-virtual-ma

### When did the user last successfully log in via SSH, and who was it?

![image](https://github.com/user-attachments/assets/bdd9abf3-f85a-4eaf-8f3f-be78ea06c469)

i used filter : `tcp`

Answer: // i dont know(( i need to analyze each packet. I will retur to this soon.

### How many unsuccessful SSH connection attempts were made by the attacker?


Answer: 
