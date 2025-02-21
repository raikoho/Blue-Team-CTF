Our SIEM alerted that AV blocked malware from running on an employee's machine. For further investigation, the incident response team quickly acquired an image of that machine. To find out how this malware got on the machine, their task is to find the entry point of the attack and trace the attacker.

`File Location: C:\Users\Hey\Desktop\ChallengeFile\Discord.7z`

## Start Invetigetion - Indicators Gathering

![image](https://github.com/user-attachments/assets/340ec5bb-8870-4931-b525-6e1ed12b6cf8)

Extracted hash value andcheck virus total. Nothing malicious was found. Seems like the disk image is realy fine.
Time to check the email.

Checked this locations:

`Outlook: C:\Users\LetsDefend\Desktop\ChallengeFile\Discord\Administrator\AppData\Local\Microsoft\Outlook`

`Thunderbird: C:\Users\LetsDefend\Desktop\ChallengeFile\Discord\Administrator\AppData\Roaming\Thunderbird\Profiles`

Nothing was found. But it seems like the victim also use this for emails:

`C:\Users\LetsDefend\Desktop\ChallengeFile\Discord\Administrator\AppData\Local\Microsoft\Windows Live Mail`

![image](https://github.com/user-attachments/assets/bdc48502-de85-46b1-beeb-a09dde7e2e32)

Lets investigate each of those.
I have found interesting fields from emails.

![image](https://github.com/user-attachments/assets/075fb6dc-acfc-4460-af17-aa3cab7a6fd0)

![image](https://github.com/user-attachments/assets/9a35b714-9992-455c-9cfc-f7fb57485980)

His name - `abdlhameed`
His discord username - `abdo8azy`
His email (attacker) is - `support@amazon.com`
Victim email is - `anaaslawadmo4kla@outlook.com`

and the file download link (probably malicious):

![image](https://github.com/user-attachments/assets/05c8b229-d922-4c79-9730-d448b6bb6ded)

filename - `8tgwESMK.exe`

## Discord Cache

Discord data is structured very similarly to the Google Chrome cache — this means that we can probably leverage ChromeCacheView from our Tools folder to perform further analysis.

![image](https://github.com/user-attachments/assets/bc658651-c8f5-4d8a-ae7d-9ab2d9aaf9ea)

I used this path to the dicords cache: `C:\Users\Example\Desktop\ChallengeFile\Administrator\AppData\Roaming\discord\Cache`

Then filtered for "message" to see if i can get any results.

![image](https://github.com/user-attachments/assets/20ae5e7d-5677-4770-8066-4506e67d02e4)

Looked for more detailed output to find attackers username `abdo8azy`. It maens their conversation.
Lets open by clicking "Open Selected json cache with".

There was possible to see:

![image](https://github.com/user-attachments/assets/1298d928-d5b4-414e-9d4f-d7ca10af7f14)

![image](https://github.com/user-attachments/assets/71919aa6-e33a-4ef2-81a5-cc81f50ec8b1)

We can see that the Attacker was trying to send invintation link:

![image](https://github.com/user-attachments/assets/b802bcbb-56ea-4703-b0d2-4d95df276d14)

Let’s go back into ChromeCacheView. This time, we are going to view the JSON file for the server channel instead of the private chat. Let’s open the cache file with the size of 1,392.

![image](https://github.com/user-attachments/assets/33f5190e-6576-47a7-b3fe-49fb4271600a)

After browsing the contents of the chat, we can see the attackers are coercing the victim to prove that he is the right candidate for the “job” by asking for the details of some (presumably) confidential research.

There was also attached URL:

![image](https://github.com/user-attachments/assets/4feacc67-a782-4939-ad47-36d52c46c531)

It’s sure that the victim uploaded the data from his own device, we just need to find it from the folder on his computer.
In my case - it is aquired image.
I found it in the `Document folder`

![image](https://github.com/user-attachments/assets/1430d79f-5ae6-4c81-a251-db4c62a8488d)

So now i can do something with the file. I have got a cache:

![image](https://github.com/user-attachments/assets/324fafb6-68ae-49dd-80c6-600307849cef)

From the ChromeCacheView i still was using Discord Cache Folder and i just applied "country" in the filter.  

![image](https://github.com/user-attachments/assets/358d5eb0-47af-4d91-8e63-33bb8595dc1e)

We can see France.

