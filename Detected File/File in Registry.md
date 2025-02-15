# Investigation Scenario

One of our IT technicians has reported an unexpected file in the registry of an employee they were assisting. 
I have been tasked with investigating using various log sources and OSINT to understand if the file is legitimate or malicious, and what it is doing.

# Questions

## 1) OSINT can be extremely useful in almost every investigation. Perform a Google search for osk.exe - what is the full name of the Windows feature associated with osk.exe?

Accessibility On-Screen Keyboard

## 2) Continue with your OSINT research. What is the expected file path for osk.exe? (Path to the folder, or full file paths are accepted)

C:\Windows\System32\osk.exe

## 3) How many events are returned based on the query to find this malicious .exe?

![image](https://github.com/user-attachments/assets/b51bb867-ba3f-407a-81e3-1cba711688d2)

49608

## 4) What is the full file path of the suspicious executable?

![image](https://github.com/user-attachments/assets/c698006e-4882-4fbf-8423-114949e41251)

Looking at these logs, we can see under the ‘Image’ field that osk.exe is NOT being stored in the C:\Windows\system32\ folder where it should be.

``C:\Users\bob.smith.WAYNECORPINC\AppData\Roaming\{35ACA89F-933F-6A5D-2776-A3589FB99832}\osk.exe``

## 5) What computer is the suspicious file running on, what is the internal IP address, and which user account is running it?

![image](https://github.com/user-attachments/assets/9216d0e1-2396-42d1-ade1-b03c04dfe76e)

It is easy to see it from the same log.

``we8105desk.waynecorpinc.local, 192.168.250.100, bob.smith``

## 6) Next it's a good idea to see if there are any network connections - what destination ports is this file connecting to?

Currently, as i am just using “osk.exe” in the search query, it will find events that have this string in any field of an event. 
To narrow this down within the Sysmon logs, i can include the Image field with the full file path value in the search query. 
i'll click on Image in the Interesting Fields panel and click on the path to the suspicious osk.exe.

![image](https://github.com/user-attachments/assets/63fa5551-7565-456d-b4e5-ca96ef6a18df)

Looking back to the Interesting Fields, i can see DestinationPort exists, with 2 values. Clicking on it will show the destination ports observed.

![image](https://github.com/user-attachments/assets/4e73d05c-eaf1-4a3e-a010-8a44f806eaf6)

6892, 80

## 7) Identify the number of unique destination IP addresses this file is connecting to the highest activity destination port.

So we need to pick "6892".
My search will be now:

```
search query is now:index="botsv1" sourcetype=xmlwineventlog osk.exe Image="C:\\Users\\bob.smith.WAYNECORPINC\\AppData\\Roaming\\{35ACA89F-933F-6A5D-2776-A3589FB99832}\\osk.exe" DestinationPort=6892
```

then use `| stats count by DestinationIp` (to identify uniq IPs):

![image](https://github.com/user-attachments/assets/9c55ba81-2cbb-4954-8131-20d2f94c9855)

16384

## 8) Find the SHA256 hash of the suspicious osk.exe

Sysmon EventID 7 logs contain the hash values of files (ImageLoaded field) that are executed.
So i will use this log.

Based on the information present in the question, and the fact we're still looking at `osk.exe`, we'll run the following search to see if it works as intended: 

```
index="botsv1" sourcetype=xmlwineventlog EventID=7 ImageLoaded=*osk.exe
```

![image](https://github.com/user-attachments/assets/31f9555b-9875-4257-b4ca-1e65bc35a5da)

37397F8D8E4B3731749094D7B7CD2CF56CACB12DD69E0131F07DD78DFF6F262B

## 9) what is the potential name of this malware?

I will check the hash on the VirusTotal.

![image](https://github.com/user-attachments/assets/748c4d26-8f92-4926-b469-88f4ee595438)

Cerber

## 10) What is the category of malware dedicated by Fortigate?

#### Addition info: 

Sysmon was useful, but let's investigate the network traffic coming from the suspicious file out to thousands of IP addresses. 
To do this we'll look at the Fortigate Unified Threat Management logs. Find something all (but one) of the osk.exe sysmon logs have in common regarding network traffic and use this in your search query.

---

From our previous searches, in Q6 we found that two destination ports were contacted, with 1 single request going to port 80, and all of the rest went to `6892`. 
We'll use this as the indicator for this traffic, because `6892` is not a standard destination port, so we are very unlikely to get other non-related events included in our search (if we wanted to be extra safe, we could even include the source IP of the system making these connections, just in case another system was also making requests to destination port 6892). The search query will be: 

```
index="botsv1" sourcetype=fortigate_utm dest_port=6892
```

Then we can use "appcat" and see:

![image](https://github.com/user-attachments/assets/6880a41e-d216-4bef-9e61-d286dd84e906)

Botnet

## 11) What is the name given to this specific malware by Fortigate?

![image](https://github.com/user-attachments/assets/86006aae-d547-4bd6-816c-94bd31ec8acb)

Cerber.Botnet

## 12) Conduct another OSINT search for the name of the malware. What is the primary function of this malware?

I googled it. 

Ransomware

## 13) let's investigate the single connection from osk.exe to a remote IP address on destination port 80 HTTP. Find the IP from the Sysmon logs and use it to search in the suricata logs - these logs have different event types, and we're interested in 'alert'. If Suricata has alerted on this activity, what is the alert.signature value?

We can use destination port 80, but this search is going to bring back a huge amount of events, because we aren't being specific enough. We could include the source IP of the system running osk.exe, but this search is also too wide, because we would get results related to any HTTP traffic over port 80.

To be as specific as possible, let's run one of our old searches again so we can find out the destination IP of the single event of osk.exe reaching out to port 80. We'll use the query 

```
index="botsv1" sourcetype=xmlwineventlog osk.exe
```

We will click on the DestinationPort field under the Interesting Fields header on the left and this time click on "80".

![image](https://github.com/user-attachments/assets/8598ca2f-6223-47b9-b162-8159a4e86efa)

Looking at the single event, we can retrieve the source IP and Destination IP values from the log. We'll use these to narrow our final search to make sure we're looking at the right Suricata logs. We need to remember that while Sysmon logs use 'SourceIp' and' DestinationIp' field names, we cant be 100% sure that Suricata will use those same names, so to work out what those fields are called let's jump in with a wide search using 

```
index="botsv1" sourcetype=suricata
```

Now that we understand the formatting, we can build our full search query to identify the suspicious port 80 HTTP traffic from osk.exe using Suricata logs: 

```
index="botsv1" sourcetype=suricata src_ip=192.168.250.100 dest_ip=54.148.194.58 dest_port=80
```

Then just + filter (alert-signature):

![image](https://github.com/user-attachments/assets/0498f9a6-c614-4d8d-bfa7-9c06b3c2a6c4)

