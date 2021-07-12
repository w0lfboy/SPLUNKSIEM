# Splunk Reports
In this repository, I utilize Splunk to design a powerful monitoring solution to protect Vandaly from security attacks.

The [Speed Test File](https://github.com/w0lfboy/splunk_reports/blob/main/csv%20files/server_speedtest.csv) has been uploaded to my local Splunk ES for analysis.

## Step 1: The Need for Speed
*Task: Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed.*

First, I created an eval command that shows the ratio between upload and download speeds: ` | eval ratio = 'UPLOAD_MEGABITS' / 'DOWNLOAD_MEGABITS' `

Then, I created a table to help me analyze when the approximate date and time of the attack occured: ` | table _time IP_ADDRESS DOWNLOAD_MEGABITS UPLOAD_MEGABITS ratio `

![18.1 hw](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.1%20hw.png)

Based on the report created, the attack occured at approximately between 12:00pm and 6:00pm on Sunday, February 23rd.  It took our system approximately 6 hours to recover to the original upload/download speed.  By using the visualization column chart seen below, I was able to interpret the data easier and visualize how the system recovered from the attack.

![18.2 hw](https://github.com/w0lfboy/splunk_reports/blob/main/images/18.2%20hw.png)

## Step 2: Are We Vulnerable?
Background:  Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities.

For more information on Nessus, read the following link: https://www.tenable.com/products/nessus


Task: Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server.


Upload the following file from the Nessus vulnerability scan.

Nessus Scan Results



Create a report that shows the count of critical vulnerabilities from the customer database server.

The database server IP is 10.11.36.23.
The field that identifies the level of vulnerabilities is severity.



Build an alert that monitors every day to see if this server has any critical vulnerabilities. If a vulnerability exists, have an alert emailed to soc@vandalay.com.


Submit a screenshot of your report and a screenshot of proof that the alert has been created.

## Step 3: Drawing the (base)line
Background:  A Vandaly server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again.
Task: Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring.


Upload the administrator login logs.

Admin Logins



When did the brute force attack occur?

Hints:

Look for the name field to find failed logins.
Note the attack lasted several hours.





Determine a baseline of normal activity and a threshold that would alert if a brute force attack is occurring.


Design an alert to check the threshold every hour and email the SOC team at SOC@vandalay.com if triggered.


Submit the answers to the questions about the brute force timing, baseline and threshold. Additionally, provide a screenshot as proof that the alert has been created.

Your Submission
In a word document, provide the following:

Answers to all questions where indicated.
Screenshots where indicated.
