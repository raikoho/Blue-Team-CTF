Possible SQL Injection Payload Detected

Event ID: 115

Alert Type: Web Attack

MITRE Technique:

T1190 - Initial Access - Exploit Public-Facing Application,

Severity: High

![image](https://github.com/user-attachments/assets/d3f89a9c-844c-4e11-abc7-db7d5f71be8d)

## Solving

I tried to decode:

![image](https://github.com/user-attachments/assets/e53f132e-3e74-4132-ac0e-a1d0fe00fee3)

it seems like source ip is malicious:

![image](https://github.com/user-attachments/assets/6bf77bee-6d97-4608-9746-3f44e523abe0)

Next step - filter logs by malicious src IP to view more:

![image](https://github.com/user-attachments/assets/cfd8aab3-21fa-4262-a992-3fd7b917db3f)

The commands were used:

```

https://172.16.17.18/search/?q=1' ORDER BY 3--+
https://172.16.17.18/search/?q=' OR 'x'='x
https://172.16.17.18/search/?q=' OR '1
https://172.16.17.18/search/?q='
https://172.16.17.18/search/?q=" OR 1 = 1 -- -
```

All pages have statuses of 500. A SQL injection was purposefully introduced. 
However, it was not successful, as there was no data leak.

But it is `True Positive`
