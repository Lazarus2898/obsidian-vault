```powershell
PS C:\htb> Import-Module activedirectory 
PS C:\htb> Get-ADTrust -Filter *

# Checking for existing trusts.
PS C:\htb> Get-DomainTrust

# Mapping the domains
PS C:\htb> Get-DomainTrustMapping

# Getting the users in the child domain
PS C:\htb> Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```

### Using NetDom
```powershell
# Getting the domain controllers
C:\htb> netdom query /domain:inlanefreight.local trust

# Getting the workstations and servers
C:\htb> netdom query /domain:inlanefreight.local workstation
```