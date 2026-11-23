# Visualizations
* Visualization 1: Failed logon attempts (All users)

Such a visualization might reveal potential brute force attacks. It's important to identify any single user with numerous failed attempts or perhaps, various users connecting to (or from) the same endpoint device. However, the current data does not point towards any such scenario. One anomaly is noticeable though. Hint: It is related to the "sql-svc1" account.
* Visualization 2: Failed logon attempts (Disabled user)

It seems that there is one incident where the user "Anni" has tried to authenticate, despite the account being disabled.
* Visualization 3: Failed logon attempts (Admin users only)

Hint: Check if all events took place on either Privileged Access Workstations (PAWs) or Domain Controllers.
* Visualization 4: RDP logon for service account

Service accounts in this environment serve a very specialized function. Do you notice anything that warrants suspicion?
* Visualization 5: User added or removed from a local group

An administrator has incorporated an individual (who is only represented by the SID value) into the "Administrators" group. Should you escalate to a Tier 2/3 analyst or consult with the IT Operations department first?
* Visualization 6: Admin logon not from PAW

Administrators should exclusively utilize PAWs for server remote connections. Should you escalate to a Tier 2/3 analyst or consult with the IT Operations department first?
* Visualization 7: SSH Logins

Be reminded that the root user account is not typically in use.

---
# Question 1
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "Failed logon attempts [All users]" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
Consult with IT Operations

# Question 2
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "Failed logon attempts [Disabled user]" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
Escalate to a Tier 2/3 analyst

# Question 3
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "Failed logon attempts [Admin users only]" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
Nothing suspicious

# Question 4
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "RDP logon for service account" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
Escalate to a Tier 2/3 analyst

# Question 5
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "User added or removed from a local group" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
Consult with IT Operations

# Question 6 
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "Admin logon not from PAW" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
Consult with IT Operations

# Question 7
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Dashboard". Review the "SSH Logins" visualization of the "SOC-Alerts" dashboard. Choose one of the following as your answer: "Nothing suspicious", "Consult with IT Operations", "Escalate to a Tier 2/3 analyst"
"Escalate to a Tier 2/3 analyst
