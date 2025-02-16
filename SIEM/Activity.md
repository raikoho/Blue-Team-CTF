I`ve been tasked with trying it out to investigate some suspicious activity, and also add some new panels to the dashboard.

## 1) Click on Dashboards and go to Splunk Investigation 4. How many Suricata alerts are there, and how many Fortigate alerts are there?

![image](https://github.com/user-attachments/assets/e465aa7a-e59e-4bc3-80f5-84cf4b05aef7)

## 2) Edit the dashboard and look at the search query for the Fortigate Alerts counter. What is the full query used to generate this number?

![image](https://github.com/user-attachments/assets/1e042c4e-dd74-4f06-9a70-356d5349a880)

index=* sourcetype=fortigate_utm level=alert | stats count as Total

## 3) What is the full query used to generate the Suricata Alerts counter?

![image](https://github.com/user-attachments/assets/ddee3947-41d5-45fe-9542-448260c5a4bd)

index=* sourcetype=suricata event_type=alert alert.category!=""| stats count as Total

## 4) Click on the Suricata alert titled 'Information Leak' to see the associated events. What is the source IP address, and what is the destination IP address?

![image](https://github.com/user-attachments/assets/1561db99-00ea-4c0f-a137-2f0f6bbcead7)

![image](https://github.com/user-attachments/assets/282a8f0f-8be2-4314-a371-1bfcee98325d)

## 5) What action did Suricata take after observing these events?

Its hard to see any fields that would tell me what actions Suricata took, so we'll click on 'Show as raw text' at the bottom of each event to change the displayed format. We can now see a field called ‘action’!

![image](https://github.com/user-attachments/assets/07d9430c-f5af-4adc-8fef-4cce9c54b872)

## 6) We know the alert category is 'Information Leak', however the specific signature can provide us with more information about this activity. What is the signature shared by both events?

![image](https://github.com/user-attachments/assets/e7abbeea-1bee-4d97-8e93-8c8679317ece)

same search filters. just see field.

## 7) Based on the logs, combine two fields to understand the full website addresses being accessed by the attacker

![image](https://github.com/user-attachments/assets/10ef44d1-3d66-47d7-8daf-76d0b977615d)

![image](https://github.com/user-attachments/assets/dcaa4a1a-7ff1-4845-823f-8c5b6c647488)

Answer: imreallynotbatman.com/phpinfo.php, imreallynotbatman.com/phpinfo.php5

## 8) What HTTP status code is returned to both of these requests, that tells us this attack was not successful?

![image](https://github.com/user-attachments/assets/805a8a0b-2718-4d51-a600-b114f3a503db)

404

## 9) Return to the Dashboard and click on the Suricata alert titled 'A Network Trojan was detected' to load this search. Modify the search query to show count of every signature field within this alert category. How many unique suricata signatures are present?

To do this we'll add the following to our search query to get the count of signature values: 

`| stats count by signature` 

Looking at the Statistics title, we can see there are 12 unique signatures that have been observed within logs for this category.

![image](https://github.com/user-attachments/assets/f32ee031-1e1d-49e8-a74e-21dcf927f376)

## 10) Search manually through Suricata logs where the HTTP status code is 200, then perform a count of each signature field to find two signatures that reference a vulnerability CVE identifier. Search this CVE on the National Vulnerability Database.- what is the CVSS Version 3 Score?

so i'll search `index="botsv1" sourcetype=suricata event_type=alert status=200` in the search filter.

Great, next we need to get the count of values in the signature field. Ie'll add the following to our search: 

`| stats count by signature`

![image](https://github.com/user-attachments/assets/74ed84d0-531e-4727-be1c-b3553bba8e00)

i've found the reference to a Common Vulnerability and Exposures identifier, used as an identification method for vulnerabilities. Next we're asked to find the score of this vulnerability, so we'll search for it on Google using the search “CVE-2014-6271 national vulnerability database”.

![image](https://github.com/user-attachments/assets/0acfddd0-ed21-4b49-88b5-696e9b1e94fb)

![image](https://github.com/user-attachments/assets/5b9f36de-0f9a-4556-b20a-b39c710c1b21)

## 11)On the Fortigate Security Alerts dashboard table click on 'MS.Windows.CMD.Reverse.Shell'. Identify the internal IP within this event, and use your SIEM skills to identify the name of this system.

`index=* sourcetype=fortigate_utm level=alert   attack="MS.Windows.CMD.Reverse.Shell"`

We can see that the internal IP address is in the "dstip" field, and is `192.168.250.70`

Because this log is from a Firewall, Fortigate has no idea what the hostname is for this system, so we'll need to use a different log source to find this.

Let's be smart about this, this alert is about abuse of the Microsoft Windows Command Prompt. It is possible that the internal system is running Windows, and should have Sysmon logs enabled (xmlwineventlog). Let's change our search query to look at that sourcetype and free-search the IP (without declaring a field name, as we don't know what format it will be).

![image](https://github.com/user-attachments/assets/38c69c5b-f7e7-43a3-9f8c-df9d6252b078)

Here is another proposed method of solving it—which many students find easier:

![image](https://github.com/user-attachments/assets/2d963bd1-86d2-4fa5-9b05-c7c2554c5cc5)

Filtering by the IP 192.168.250.70 will indeed yield multiple hostnames.

However, if the user includes the attacker's IP address in their search, they will obtain a single result and the answer to question 11.

```
index=* sourcetype=xmlwineventlog 23.22.63.114 192.168.250.70 | stats count by SourceHostname | sort - count
```

## 12) Go back to the Fortigate Security Events table and click on 'Apache.Roller.OGNL.Injection.Remote.Code.Execution'. Find the reference field in the log and open the URL on your host machine. What is the Affected Products text, and the CVE identifier?

![image](https://github.com/user-attachments/assets/5d4f8a50-2e52-42f8-9db3-19ec714e5341)

I visited it.

Apache Software Foundation Apache Roller prior to 5.0.2, CVE-2013-4212

## 13) On the dashboard consider the Fortigate category with the highest number of events. Try to find the version of the scanning tool being used, looking at Fortigate logs then Suricata logs.

This one has the most events inside:

![image](https://github.com/user-attachments/assets/5ae03c3a-41ef-48f0-8cc7-7e52475a5233)

As we can't find anything helpful in the Fortigate logs, let's pivot to the Suricata logs instead. To do this we'll change our sourcetype to Suricata and free-search for ‘Acunetix’ as this is the name of the scanner.

```
index=* sourcetype=suricata Acunetix
```

![image](https://github.com/user-attachments/assets/10613f9c-e5c2-4bf5-9e6f-964f6dfe6af4)

## 14) Investigate Suricata 'alert' logs to understand how they present the severity of the alert. Create a search query that gets the count of events based on each severity rating. When you having a working query click on 'Save As > Existing Dashboard' and select the Splunk Investigation 4 dashboard. Edit the dashboard and click on 'Select visualization' on the panel you just added to change it to a pie chart (feel free to add an appropriate title!). Hover your mouse over the 'High' section of the pie chart, what is the count%?.

As start we have this:

![image](https://github.com/user-attachments/assets/95a01db3-bfbf-4d99-afbf-721665505788)

Now that we know the severity field is called alert.severity, we could perform a stats count for each severity. Interestingly, there is another field called "severity" that is using low, high, medium ratings, which is easier for us to understand than numbers, so we'll use this instead.

![image](https://github.com/user-attachments/assets/e870eac8-f6d4-4543-90dc-da17a5e3559b)

![image](https://github.com/user-attachments/assets/3974387a-e984-4cba-aa4f-2b9cef6ba153)

Now that our search works, let's save it to our Splunk Investigation 4 dashboard using the "Save As" button. As existing dashboard!!!

We now see the search at the bottom of our dashboard, however by default it is in the Table visualisation, and we want to change this to a Pie Chart. We'll click "Edit" in the top right of the dashboard then click on the ‘Select Visualization’ button of our new panel.

![image](https://github.com/user-attachments/assets/298efa51-d684-4ef5-81bf-f720680f400b)

then we cn hover the cursor on the section with high severity. 

28.824 %

## 15) Complete the same steps above but for Fortigate_UTM logs, creating a pie chart based on severity. If you want to keep things neat, you can drag your new pie chart next to your Suricata one! What is the count% of critical alerts?

First we need to understand the field name that holds the severity rating for these logs. We'll perform a generic search for `fortigate_utm` alert logs, and quickly find a field called "severity" - perfect! Let's create a search to get the counts per severity value.

![image](https://github.com/user-attachments/assets/35b4af10-499c-4a71-9dd2-f450e6744ef3)

Same procedure as prior.

![image](https://github.com/user-attachments/assets/7aee06b9-debd-4900-83f6-94642cb006e4)
