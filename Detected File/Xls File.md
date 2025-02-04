## Detected Suspicious Xls File

Event ID: 77
Event Time: Mar, 13, 2021, 08:20 PM
Rule Name: SOC138 - Detected Suspicious Xls File
Alert Type: Malware
MITRE Technique:
T1112 - Defense Evasion - Modify Registry,
Severity: Medium
Security Analyst: Bohdan Misonh

![image](https://github.com/user-attachments/assets/b86fa7b9-9781-4fac-8489-731d10df5a03)

1) I have found Endpoint:

![image](https://github.com/user-attachments/assets/ee277ddd-43fa-40ef-bd7f-32913534eb44)

So i need to check everything on this destination.

Nothing was found.

![image](https://github.com/user-attachments/assets/d2734011-d99a-4815-9b94-2896e551f4ba)

2) I attempted to search more detailed about the file and its asociation:

![image](https://github.com/user-attachments/assets/8fffb3cc-70c3-4635-a91b-738a524c5fb5)

3) using the playbook:

![image](https://github.com/user-attachments/assets/8a3a128c-ac80-4817-9931-51bb2ba6f7cc)

It was actualy cleaned, because it was not possible to fin both: the hash and the filename. - empty
![image](https://github.com/user-attachments/assets/ae33e134-8f6e-4a81-a02a-d21c1f30b755)

![image](https://github.com/user-attachments/assets/756c41ca-bed0-47ac-b689-1244b2075e53)

so it was true positive. And i just closed and contained endpoint.
