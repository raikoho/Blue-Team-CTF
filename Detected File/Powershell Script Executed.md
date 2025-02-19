## Suspicious Powershell Script Executed

Event ID: 101

Event Time: Sep, 05, 2021, 12:43 PM

Rule Name: SOC153 - Suspicious Powershell Script Executed

Alert Type: Malware

MITRE Technique:

T1059.001 - Execution - Command and Scripting Interpreter: PowerShell,

Severity: High

## Start

![image](https://github.com/user-attachments/assets/fecf82c2-d69c-4928-9d28-f7488947f8d7)

I decieded to Find the device in the alarm details on the 'Endpoint Security' page and access the device.

![image](https://github.com/user-attachments/assets/0793e618-2c2d-4e96-8011-3e15def43978)

So first of all i need to aquire the script and check virustotal:

![image](https://github.com/user-attachments/assets/22c69d82-e48e-4043-a2bc-5d181dbe7795)

I used this base64 and Gunzip to decode it:

![image](https://github.com/user-attachments/assets/b016b467-1f67-4dcb-816d-d72762b98a07)

In the script I found code that looked like it was encoded XOR. so now it's time to decode this. 
(Since we know that the decoded data is bitwise XORed using 35 as the key, we are going to use the XOR recipe to decode it.)

![image](https://github.com/user-attachments/assets/40e08567-a5bf-47a2-a1b4-c3587cf2687d)

Now it's worth saving this to a new file. And check it again with virustotal. Now it's obvious that the script launches an executable program and is similar to CobaltStrike.

It’s now confirmed that the file is indeed malicious and the alert is a true positive

![image](https://github.com/user-attachments/assets/20749713-f02a-43e6-9c0c-519e6d56f1d4)

This is a malware program because it has a principle similar to a Trojan. But it is tagged as a malware.

![image](https://github.com/user-attachments/assets/f12429c1-f89a-47fd-a5af-a190cb245a48)

## Analysis:

`netstat -an | findstr "LISTEN"`

i used this to check all ports that are listening for connection.

![image](https://github.com/user-attachments/assets/e7887485-20d2-46e6-ad28-5b6589505731)

Port 3389 usually used for remote access to a computer.

We will now delve further into the event logs that are connected to RDP access, expanding on the evidence we’ve gathered. From the SIEM alert, we know that the attack utilized an Administrator account. Event ID: 1149 from the Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational log recorded 4 successful RDP connections on the day of the incident:

'''
3 connections from the IP address 3.16.42.241 for the User account Administrator
1 connection from the IP address 94.232.41.158 for the User account A
'''

The connection from IP address 3.16.42.241 as Administrator aligns with the information provided in the alert. By reviewing the TCP connections to the RDP port (Event ID: 261) along with the successful connection logs (Event ID: 1149), we see signs of a brute force attack. Security logs with audit failures (Event ID: 4625) further corroborate this.

This confirms that the initial access occurred via brute forcing an open and unsecured remote service port accessible over the Internet.

Execution: To investigate the attacker’s actions, we can examine Sysmon logs, if available, to track dropped files and initiated processes. Event ID: 11 in Sysmon, which refers to file creation, revealed several noteworthy events:

'''
C:\Users\Public\Documents\new1.bat
C:\Users\Administrator\Downloads\Advanced_Port_Scanner_2.5.3869.exe:Zone.Identifier
C:\Users\Administrator\Downloads\nc111nt
C:\Users\Public\Documents\endpoint.ps1
'''

The file new1.bat appears to be missing, likely deleted from the system. Event ID 1, which logs process creation, indicates that it was executed by cmd.exe under the Administrator account. By tracking its Process ID, we can observe its child processes and infer what it was doing. These processes appear to be collecting system data, running commands like:

'''
net user
systeminfo
ipconfig /all
netstat -an
net config workstation
tasklist
and others...
'''

Additionally, processes using findstr are searching for strings within files in the Documents directory that contain domain names. This suggests an attempt to retrieve credentials stored in files containing these domain names.

Filtering Sysmon for Event ID 3 that relates to detected network connections, we can see that AdvancedPortScanner was used to perform port scanning likely for lateral movement over the internal network.

Going through the process creation events in Sysmon logs, we are able to find the process that executes the endpoint.ps1 script that triggered the SIEM alert.

![image](https://github.com/user-attachments/assets/170ec610-cb88-4022-a8c0-9bbe45383d66)

we can now see the process that executes nc111nt, and looking through the various evidences it appears to be netcat. Netcat is often used by attackers for data exfiltration, we see the command that was run to connect to the IP address 3.16.42.241 on port 4444. Which is the same IP address as the attacker that brute forced the Administrator account.

![image](https://github.com/user-attachments/assets/44b630be-7920-4bc9-9e1a-0efe359bfe46)

Sysmon logs for detected network connections confirms that the connection was successful.
Event Logging was disabled at 1:16 PM which makes tracking the attackers actions after this event more difficult.

![image](https://github.com/user-attachments/assets/a79594c0-8e7f-47e9-8e59-52a18057ff08)

## Conclusion:

Disconnect the Endpoint from the network. And take all measures to increase security.
