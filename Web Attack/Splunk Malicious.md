Conduct analysis of malicious activity using Splunk.

Investigation Scenario

Our website has shown signs of high resource usage, but it doesn't look like a distributed denial-of-service attack, because the request are coming from one single IP address. They seem to be performing a port scan and trying to access different resources on the website - it could be a vulnerability scanner. We need you to investigate and gather information about this activity, include where it's coming from, what tool are they using, and the resources they have accessed. We run routine checks, but if they find something we haven't seen before, this could get bad really quick.


Instead of looking through thousands of web logs we can look at logs from the Fortigate firewall, specifically the Unified Threat Management solution, so make sure to set your sourcetype as fortigate_utm. 
You should search for our domain name as a string, "imreallynotbatman.com" and the string "vulnerability" to find events related to this activity.

## Why i use "fortigate_utm"?

I googled it:

`sourcetype=fortigate_utm` in Splunk means that the source of the logs is FortiGate UTM (Unified Threat Management).

What does this mean?
FortiGate UTM is a network device from Fortinet that performs the functions of a firewall, IPS, antivirus, web filtering, antispam, etc.
Its logs can contain data about traffic filtering, attack blocking, intrusion attempts, and suspicious activity.
Splunk uses a special sourcetype=fortigate_utm to correctly parse logs from FortiGate.

## Finding the scanner

```
index="botsv1" sourcetype=fortigate_utm url_domain="imreallynotbatman.com"
```

![image](https://github.com/user-attachments/assets/bd0e417b-c296-419b-ad97-74acf7c734d3)

Answer: Acunetix

## What is the source IP? (the internal address for our web server)

Now when I add this scanner to the filter, I only have one IP address to choose from, indicating that this is the real IP address of the attacker who used this scanner.

![image](https://github.com/user-attachments/assets/6730a91b-79a7-4f8c-a545-7d45ea61d156)

Answer: 40.80.148.42

## What is the destination IP? (the internal address for our web server)

Same strategy as prior.

![image](https://github.com/user-attachments/assets/cf43692a-9bae-4189-b1ae-bc8d7bb7532c)

Answer: 192.168.250.70

## Fortigate UTM provides enrichment, and can tell us the source IP country based on a lookup. What country is the scanning IP associated with?

![image](https://github.com/user-attachments/assets/c4720114-31c3-464b-9bd1-fb9cea361881)

Answer: United States 

## What is the log 'time' field value for the first Fortigate UTM log referencing the vulnerability scan, on 8/10/16, in the format HH:MM:SS?

![image](https://github.com/user-attachments/assets/d725cf57-2fe2-4563-b7ff-febfa6034097)

I just sorted everything and used "tail". Then it is easy to see the time:

Answer: 15:36:45
