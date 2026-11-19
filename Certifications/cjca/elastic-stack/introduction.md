# Introduction

### Question 1
Navigate to http://[Target IP]:5601, click on the side navigation toggle, and click on "Discover". Then, click on the calendar icon, specify "last 15 years", and click on "Apply". Finally, choose the "windows*" index pattern. Now, execute the KQL query that is mentioned in the "Comparison Operators" part of this section and enter the username of the disabled account as your answer. Just the username; no need to account for the domain.

1. Go to the IP address
2. Side bar analytics/discover
3. Change `zeek` to `windows*`
4. Go to the Calender icon in the top right. Sort by 15 and years.
5. Then in the KQL put in this.
```KQL
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072 AND @timestamp >= "2023-03-03T00:00:00.000Z" AND @timestamp <= "2023-03-06T23:59:59.999Z"
```
Side arrow beside the return to drop down the menu
Form there you can find the user name.
