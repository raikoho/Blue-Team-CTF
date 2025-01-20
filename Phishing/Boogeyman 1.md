## Artefacts

For the investigation proper, you will be provided with the following artefacts:

- Copy of the phishing email (dump.eml)
- Powershell Logs from Julianne's workstation (powershell.json)
- Packet capture from the same workstation (capture.pcapng)
- Note: The powershell.json file contains JSON-formatted PowerShell logs extracted from its original evtx file via the evtx2json tool.

You may find these files in the `/home/ubuntu/Desktop/artefacts` directory.

## Tools

- Thunderbird - a free and open-source cross-platform email client.
- LNKParse3 - a python package for forensics of a binary file with LNK extension.
- Wireshark - GUI-based packet analyser.
- Tshark - CLI-based Wireshark. 
- jq - a lightweight and flexible command-line JSON processor.

## Start

Let's start with analysing the `dump.eml` file located in the artefacts directory. There are two ways to analyse the headers and rebuild the attachment:

- command-line tools such as `cat`, `grep`, `base64`, and `sed`.
- open it via `Thunderbird`.

### What is the email address used to send the phishing email?

agriffin@bpakcaging.xyz

### What is the email address of the victim?

julianne.westcott@hotmail.com

![Скриншот 20-01-2025 12 51 13](https://github.com/user-attachments/assets/6d9b982e-c8d2-4ab2-b351-7abfdee1da14)

### What is the name of the third-party mail relay service used by the attacker based on the DKIM-Signature and List-Unsubscribe headers?

![Скриншот 20-01-2025 12 55 23](https://github.com/user-attachments/assets/d5fcf9d7-feec-4a00-9823-1456f641f6ae)

-------------

## Next Step - Powershell

With the following discoveries, we should now proceed with analysing the PowerShell logs to uncover the potential impact of the attack:

- Using the previous findings, we can start our analysis by searching the execution of the initial payload in the PowerShell logs.
- Since the given data is JSON, we can parse it in CLI using the jq command.
- Note that some logs are redundant and do not contain any critical information; hence can be ignored.

Use THIS cheetsheet:

![Скриншот 20-01-2025 13 10 13](https://github.com/user-attachments/assets/f6bd5ddc-ba24-4eb5-88e0-cd19c40fec07)


### What are the domains used by the attacker for file hosting and C2? Provide the domains in alphabetical order

``cat powershell.json | jq ‘{ScriptBlockText}’``

Answer: cdn.bpakcaging.xyz,files.bpakcaging.xyz

### What is the name of the enumeration tool downloaded by the attacker?

we have a hint ‘download’ so grep for it.

### What is the file accessed by the attacker using the downloaded sq3.exe binary? Provide the full file path with escaped backslashes.

grep for sq3.exe, still you need a complete path, also grep for cd seperately to know the path of the particular user

### What software whas used in previous question?

Look at the path: Microsoft Sticky Notes

### What is the name of the exfiltrated file?

cat powershell.json | jq ‘{ScriptBlockText}’ | grep destination

Answer: protected_data.kdbx

## Next Step - Network Traffic Analysis

Now:

- Utilise the domains and ports discovered from the previous task.
- All commands executed by the attacker and all command outputs were logged and stored in the packet capture.
- Follow the streams of the notable commands discovered from PowerShell logs.
- Based on the PowerShell logs, we can retrieve the contents of the exfiltrated data by understanding how it was encoded and extracted.

### What software is used by the attacker to host its presumed file/payload server?

http.host == files.bpakcaging.xyz

right click any packet click follow http stream

Answer - python

### What HTTP method is used by the C2 for the output of the commands executed by the attacker?

http.host == cdn.bpakcaging.xyz:8080

Answer - POST

### What is the password of the exfiltrated file?

![image](https://github.com/user-attachments/assets/84378932-d585-4f3e-8809-d37f9be14f78)

you’ll get a http packet ,as you move down a little you can see a http packet with POST method, we know that this is the method used by c2 server ,this packet has encoded value decode it using cyberchef

Answer - %p⁹³!lL^Mz47E2GaT^y

### What is the credit card number stored inside the exfiltrated file?

```
tshark -r capture.pcapng -Y ‘dns’ -T fields -e dns.qry.name | grep “.bpakcaging.xyz” | cut -f1 -d ‘.’| grep -v -e “files” -e “cdn” | uniq | tr -d ‘\n’ > output.txt

cat output.txt | xxd -r -p > secret.kdbx
```






