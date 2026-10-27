# Questions
1. What vulnerable application is running?
2. What port is this application running on?
3. What version of the application is in use?
4. Exploit the application to obtain a shell and submit the contents of the flag.txt file on the Administrator desktop.

# Walkthrough
Starting off with an `nmap`.
```bash
nmap -sC -sV -v 10.129.202.80 

Not shown: 990 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_09-01-21  08:07AM       <DIR>          website_backup
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Freight Logistics, Inc
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: APPS-SKILLS1
|   NetBIOS_Domain_Name: APPS-SKILLS1
|   NetBIOS_Computer_Name: APPS-SKILLS1
|   DNS_Domain_Name: APPS-SKILLS1
|   DNS_Computer_Name: APPS-SKILLS1
|   Product_Version: 10.0.17763
|_  System_Time: 2026-10-27T01:53:49+00:00
| ssl-cert: Subject: commonName=APPS-SKILLS1
| Issuer: commonName=APPS-SKILLS1
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-10-26T01:50:24
| Not valid after:  2027-04-27T01:50:24
| MD5:     1865 5b27 3a65 c983 6601 f25d 6d97 a609
| SHA-1:   5116 b6ad 373c 9027 99a0 28f2 552e b3b7 af55 55cd
|_SHA-256: 0143 0625 982a c4d2 f6fb 7cc8 b06d cdb7 715a 4ff8 5608 0051 6f79 f6c9 bade ec33
|_ssl-date: 2026-10-27T01:53:59+00:00; +1h00m00s from scanner time.
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
8000/tcp open  http          Jetty 9.4.42.v20210604
|_http-favicon: Unknown favicon MD5: 23E8C7BD78E8CD826C5A6073B15068B1
|_http-server-header: Jetty(9.4.42.v20210604)
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Site doesnt have a title (text/html;charset=utf-8).
8009/tcp open  ajp13         Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8080/tcp open  http          Apache Tomcat/Coyote JSP engine 1.1
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/9.0.0.M1
|_http-favicon: Apache Tomcat
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 59m59s, deviation: 0s, median: 59m59s
| smb2-time: 
|   date: 2026-10-27T01:53:51
|_  start_date: N/A
```

Starting off you can find the `TomCat Apache` running.

Port : `8080`
Version: `9.0.0.M1`


Finally, looking into the versions I found `exploit(windows/http/tomcat_cgi_cmdlineargs)`
Setting the correct `RHOST, LHOST, RPORT, LPORT and TARGETURI`. Shell was grabbed and flag conquered.