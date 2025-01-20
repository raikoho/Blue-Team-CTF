## The Greenholt Phish

### What is the Transfer Reference Number listed in the email's Subject?

![image](https://github.com/user-attachments/assets/f7d810d2-a112-492a-80be-e7ca38d454ec)
Open the EML file with Thunderbird.

![image](https://github.com/user-attachments/assets/a5ab0b22-6a10-4f42-9836-82a2e70674ed)

Easy to see that Answer: 09674321

### Who is the email from?

From previous screenshot - Mr. James Jackson

### What is his email address?

From previous screenshot - info@mutawamarine.com

### What email address will receive a reply to this email? 

From previous screenshot - "Reply to" header.

### What is the Originating IP?

Open challenge.eml file and search.

![image](https://github.com/user-attachments/assets/76266600-2f1d-48e7-b68d-6bc15ef771de)

### Who is the owner of the Originating IP? (Do not include the "." in your answer.)

look up the domain - HostWinds LLC

### What is the SPF record for the Return-Path domain?

I used mxtoolbox and copied the email header and pasted in here for analysis.

![image](https://github.com/user-attachments/assets/46bdbc49-b589-4987-980e-9944edf8a902)

v=spf1 include:spf.protection.outlook.com -all

### What is the DMARC record for the Return-Path domain?

See previous screenshot: v=DMARC1; p=quarantine; fo=1

### What is the name of the attachment?

I looked at the .eml file again and find attachments:

![image](https://github.com/user-attachments/assets/11a9c992-672d-49c8-bd2b-5b66b873c173)


### What is the SHA256 hash of the file attachment?

Save this file. And use md5sum <Filename>

![image](https://github.com/user-attachments/assets/528acab6-e8cc-4bea-8818-b72fa30c8dca)

### What is the attachments file size? (Don't forget to add "KB" to your answer, NUM KB)

copy the hash value and looked it up at virustotal and found the size

![image](https://github.com/user-attachments/assets/273fba8d-8ae4-4be1-9b2b-fc98eb462ad5)

### What is the actual file extension of the attachment?

Heading over to the details tab - Virustotal, the file type is RAR.
