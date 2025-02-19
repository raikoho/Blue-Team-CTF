![image](https://github.com/user-attachments/assets/01cf07fa-4a11-4bac-8b64-6ae5a465fc85)A person working in the accounting department wanted to add a browser extension, but it was deleted from his device because it was perceived as harmful by AVs.

I just need to analyze the situation by analyzing this suspicious browser extension.

`File link: /root/Desktop/Files/FinanceEYEfeeder.crx`

## Which browser supports this extension?

Answer: Google Chrome

## What is the name of the main file which contains metadata?

I need to unzip it. So i used:

`unzip FinanceEYEfeeder.crx -d output_folder`

![image](https://github.com/user-attachments/assets/b996670e-f824-457f-925e-d22a9f1b745a)

Answer: manifest.json

## How many js files are there? (Answer should be numerical)

just see the screenshot above.

Answer: 2

## Go to crxcavator.io and check if this browser extension has already been analyzed by searching its name. Is it known to the community? 

Answer: No

## Download and install ExtAnalysis. Is the author of the extension known? 

He is not stored in "manifest.json"

Answer: No

## Often there are URLs and domains in malicious extensions. Using ExtAnlaylsis, check the ‘URLs and Domains’ tab How many URLs & Domains are listed? (Answer should be numerical)

There were 2.

![image](https://github.com/user-attachments/assets/b20bcc8e-9081-4a81-9404-00328a7a5af5)

Answer: 2

## Find the piece of code that uses an evasion technique. Analyse it, what type of systems is it attempting to evade?

Answer: virtual machine

## If this type of system is detected what function is triggered in its response?

Answer: chrome.processes.terminate(0)

## What keyword in a user visited URL will trigger the if condition statement in the code?

Answer: login

## Based on the analysis of the content.js, what type of malware is this?

Answer: keylogger

## Which domain/URL will data be sent to?

Answer: https://google-analytics-cm.com/analytics-3032344.txt

## As a remediation measure, what type of credential would you recommend all affected users to reset immediately?

Answer: password

