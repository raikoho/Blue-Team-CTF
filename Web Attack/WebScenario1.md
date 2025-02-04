Challenge File: /root/Desktop/ChallengeFile/access.log

## Walkthroughs:
Investigate Web attack. Letsdefend challenge #blueteam
DFIR - Investigate Web Attack
Investigate Web Attack - LetsDefend Challenge

------

So the log file was damaged or corupted. It is not possible to open it with WireShark. The only way - text editors.
![Скриншот 04-02-2025 12 37 15](https://github.com/user-attachments/assets/544d1640-6ca5-46cc-bfdb-c21e0b28733a)

## Questions

### Which automated scan tool did attacker use for web reconnaissance?

![image](https://github.com/user-attachments/assets/08fe8075-b424-4ec5-bd9d-00b0038d5f5c)

i just saw it without any filters.
Answer: Nikto

### After web reconnaissance activity, which technique did attacker use for directory listing discovery?

Answer: Directory Brute Force

### What is the third attack type after directory listing discovery?

Answer: Brute Force

### Is the third attack successful?

![image](https://github.com/user-attachments/assets/d85c0c89-0bc7-4336-9f45-32b402215bc7)

I saw 302 redirect from the clean login.php page, so..

Answer: yes

### What is the name of fourth attack?

![image](https://github.com/user-attachments/assets/e422a37c-7bfe-41df-bc6e-f440701fc2c4)

Answer: Code Injection

### What is the first payload for 4th attack?

Answer: whoami

### Is there any persistency clue for the victim machine in the log file ? If yes, what is the related payload?

Answer: %27net%20user%20hacker%20Asd123!!%20/add%27

![image](https://github.com/user-attachments/assets/9c0b0d31-3e84-4596-a69c-0a01efcb4e56)
