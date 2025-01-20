As an IT department personnel of SwiftSpend Financial, one of your responsibilities is to support your fellow employees with their technical concerns. While everything seemed ordinary and mundane, this gradually changed when several employees from various departments started reporting an unusual email they had received. Unfortunately, some had already submitted their credentials and could no longer log in.

## You now proceeded to investigate what is going on by:

- Analysing the email samples provided by your colleagues.
- Analysing the phishing URL(s) by browsing it using Firefox.
- Retrieving the phishing kit used by the adversary.
- Using CTI-related tooling to gather more information about the adversary.
- Analysing the phishing kit to gather more information about the adversary.

## Questions and Walkthrough:

### Who is the individual who received an email attachment containing a PDF?

![image](https://github.com/user-attachments/assets/cfc59e4f-b784-4461-92d1-d3958a572c36)

William McClean

### What email address was used by the adversary to send the phishing emails?

![image](https://github.com/user-attachments/assets/0ace94d2-0803-49ac-b134-b31ec42851f3)


### What is the redirection URL to the phishing page for the individual Zoe Duncan? (defanged format)

![image](https://github.com/user-attachments/assets/55fe11fa-6c9d-4d58-a868-11f58ecdaf3a)


### What is the URL to the .zip archive of the phishing kit? (defanged format)

by shortening the URL to “http://kennaroads.buzz/data/," we successfully accessed the page containing the zip file and proceeded to download it.
![image](https://github.com/user-attachments/assets/b0f03210-55e3-4000-b537-f03a01ccbb8e)


### What is the SHA256 hash of the phishing kit archive?

![image](https://github.com/user-attachments/assets/50cf5a14-0bf7-4c7f-9fbd-7d5f22c794dd)


### When was the phishing kit archive first submitted? (format: YYYY-MM-DD HH:MM:SS UTC)

![image](https://github.com/user-attachments/assets/e180394b-e58d-43c9-822a-dd2d56b99eb6)

Go to the Virustotal

### What was the email address of the user who submitted their password twice?

To retrieve this information, access the logs file. Follow the link provided in question 4, navigate to the update365 directory, and locate the log.txt file. Upon analysis, it becomes evident that michael.ascot@swiftspend.finance has submitted their password on two occasions.


### What was the email address used by the adversary to collect compromised credentials?

``grep -r ".com"``

![image](https://github.com/user-attachments/assets/d49f6c0c-3df4-4aa5-bd9b-201784201d94)

### The adversary used other email addresses in the obtained phishing kit. What is the email address that ends in "@gmail.com"?

``grep -r "gmail.com"``

![image](https://github.com/user-attachments/assets/cba4a2d7-f046-4f9d-8e6f-7804b6911542)

### What is the hidden flag?

We need to add a flag.txt at the end of this.

hence the final url is http://kennaroads.buzz/data/Update365/office365/flag.txt
