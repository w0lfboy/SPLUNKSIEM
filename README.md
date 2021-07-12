# Splunk Reports
In this repository, I utilize Splunk ES to design a powerful monitoring solution to protect and notify Vandaly of any possible security attacks.

- The [Speed Test File](https://github.com/w0lfboy/splunk_reports/blob/main/csv%20files/server_speedtest.csv) has been uploaded to my local Splunk ES for analysis.
- The [Nessus Logs File](https://github.com/w0lfboy/splunk_reports/blob/main/csv%20files/nessus_logs.csv) has been uploaded to my local Splunk ES for analysis.
- The [Administrator Logs File](https://github.com/w0lfboy/splunk_reports/blob/main/csv%20files/Administrator_logs.csv) has been uploaded to my local Splunk ES for analysis.


## Step 1: The Need for Speed
*Task: Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed.*

First, I created an eval command that shows the ratio between upload and download speeds: 
 ` | eval ratio = 'UPLOAD_MEGABITS' / 'DOWNLOAD_MEGABITS' `

Then, I created a table to help me analyze when the approximate date and time of the attack occured: 
 ` | table _time IP_ADDRESS DOWNLOAD_MEGABITS UPLOAD_MEGABITS ratio `

![18.1 hw](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.1%20hw.png)

Based on the report created, the attack occured at approximately between 12:00pm and 6:00pm on Sunday, February 23rd.  It took our system approximately 6 hours to recover to the original upload/download speed.  By using the visualization column chart seen below, I was able to interpret the data easier and visualize how the system recovered from the attack.

![18.2 hw](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.2%20hw.png)

## Step 2: Are We Vulnerable?
*Background:  Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities.*

*Task: Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server.*

To create a report that determines critical vulnerabilities that exist on the customer IP `10.11.36.23` we use the following search:
`source="nessus_logs.csv" host="nessus logs" sourcetype="csv" dest_ip="10.11.36.23" severity=critical | stats count by dest_ip,severity`

![18.3](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.3.png)

This shows a total of **49 critical** vulnerabilities on the customer IP 10.11.36.23.

Now that we have the search set up, we can create an alert that notifies the SOC of any critical vulnerabilities on the server.  The alert will monitor the IP daily and will email soc@vandalay.com if there are any vulnerabilities.

![18.4.png](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.4.png)
![18.5.png](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.5.png)


## Step 3: Drawing the (base)line
*Background:  A Vandaly server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again.*

*Task: Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring.*

Just by looking at the different name fields, we can see that there was definitely a brute force attack that was possibly successful.

![18.6](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.6.png)

Many, if not all of the fields are worth investigating further.  `A logon was attempted using explicit credentials` could be referring to multiple different brute force attacks of administrator accounts.  `Special privileges assigned to new logon` would possibly be referring to privilege escalation of multiple accounts. Since we are only tasked with the brute force attack, we will investigate that first, but it would be our due duty to continue to investigate the other attacks that occured *during* the brute force attack.

By searching be `source="Administrator_logs.csv" host="60fbd6d495dd" sourcetype="csv" name="An account failed to log on"` we can determine that the brute force attack occurred between 8:00am-2:00pm on Friday, February 21st.  We can tell that a brute force attack was happening because our baseline for incorrect logins is 25 attempts per hour and betweeen the hours of 8:00am-2:00pm we had a range of 34-124 incorrect login attempts per hour.

![18.7.png](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.7.png)

Next, we will design an alert that will check against our threshold every hour for incorrect login attempts so that the SOC team will be notified.

![18.8.png](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.8.png)

![18.9.png](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.9.png)

Our work is done!! Now we can go back and continue to investigate the suspicious activity involved with the brute force attack. :)
