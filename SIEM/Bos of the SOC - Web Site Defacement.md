![image](https://github.com/user-attachments/assets/e55eccdd-10c2-460f-9b15-d8a83bfab996)

Count all events per index:
![image](https://github.com/user-attachments/assets/0047eca3-e5cb-408d-92cf-18e02f19fca1)

Determine sources, endpoints, firewalls, IPS sources etc...
![image](https://github.com/user-attachments/assets/6eb051c6-5b33-401b-95f0-945d07cc662f)

# Questions:

## What is the likely IPv4 address of someone from the Polson1vy group scanning imreallynotbatman.com for web application vulnerabilities?

![image](https://github.com/user-attachments/assets/3d0159ce-4fa3-4b9d-a197-3b3873c062f1)

I saw interesting filter "action=blocked" - maybe because of using scanner, it is possible to force with some packets blocking.
So add this filter also. 
We have only 1 IP adress have blocked.
So now we can see what volnurability scanner was used:

![image](https://github.com/user-attachments/assets/6aba81e1-c47d-48b1-b029-04ae6cf1e314)

As we see on the screenshot above, `srcip = 40.80.148.42` is the answer.
this is also clear that `192.168.250.1` is the IP of web server.

## What scanner of vulnerabilities was used?

Answer on the screenshort above - Acunetix.

--------------------------------------------

Now filter as screenshot below. We can see strange useragent, cookie or commands fields. Its scanners work. It is tried to implement volnurabilities.

![image](https://github.com/user-attachments/assets/6f30f7ef-91ae-4e13-a80e-3e372051d13a)

Also use the suricata source type (its IDS):
![image](https://github.com/user-attachments/assets/aac3168d-27dd-499a-9868-a6e5e8118f9e)

![image](https://github.com/user-attachments/assets/99af8eca-0358-4fd3-9799-ac1adf11eabe)

## What content management system is imreallynotbatman.com likely using?

![image](https://github.com/user-attachments/assets/e3a16301-7db2-44bf-bb34-64ec20a23385)

![image](https://github.com/user-attachments/assets/397bf1e2-2a7c-4bae-bcb8-d3f0269ec83a)

Answer - Joomla

## What IPv4 was attempting to bruteforce attack against imreallynotbatman.com website?

![image](https://github.com/user-attachments/assets/b913ffe9-c9b4-4ad5-baf1-f3da7052eea7)

Try to check the first IP and here we go:

![image](https://github.com/user-attachments/assets/213b025e-8370-4ee9-99be-ae45d86eb2d1)

## What is the name of the malicious file that was downloaded the imreallynotbatman.com website?
Answer guidance: For example "notepad.exe" or "favicon.ico"

![image](https://github.com/user-attachments/assets/8e68fdf4-1053-4f06-b213-ab6c878666b9)

It was easy to spot "3791.exe"

Next can be a good practic to clear the search from filter, except index, and just search for "3791.exe" - as keyword - to view more events.

## What is the name of the file that defaced the imreallynotbatman.com website? Please submit only the name of the file with extension?
Answer guidance: For example "notepad.exe" or "favicon.ico"

![image](https://github.com/user-attachments/assets/cae68458-deb6-4e50-91c7-3ff418b21d51)

![image](https://github.com/user-attachments/assets/d4c1c61d-19ed-40d3-a3b3-13264a386c3b)

Answer - something like /batman.jpg . Just super lazy to type. See screenshot above.

## This attack used dynamic DNS to resolve to the malicious IP. What fully qualified domain name (FQDN) is associated with this attack?

![image](https://github.com/user-attachments/assets/78cc274f-64ee-4aa3-b49e-ee5191cb8af4)

the filters the same as previous question.

## What was the first bruteforce password used?

![image](https://github.com/user-attachments/assets/d111004c-c3c1-4d7c-b251-44c71d3b4d0f)

![image](https://github.com/user-attachments/assets/a3a4a072-1241-49e6-9c17-084b8f75e4d2)

## What was the correct password for admin access to the 'content management system running "imreallynotbatman.com"?

It will be the most recent event from the list. Se screenshot above.

Answer - batman

## How many seconds elapsed between the time the brute force password scan identified the correct password and the compromised login?
Answer guidance: Round to 2 decimal places.

![image](https://github.com/user-attachments/assets/bda2e237-c3cb-48cf-9a0f-95c03b57af32)

Add calculation filter:

![image](https://github.com/user-attachments/assets/2308f30e-5028-4ef2-b634-9cfad80d3c63)

## How many uniq password was attempted in the bruteforce attack?

use `dedup`

![image](https://github.com/user-attachments/assets/ff8057a2-eff2-41ff-b6c4-2a23f7187f53)
