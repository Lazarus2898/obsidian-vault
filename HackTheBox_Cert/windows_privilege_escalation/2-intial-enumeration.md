# Situational Awareness
### Network Information
Staring with the Network enumeration.

```cmd
ipconfig /all
arp -a
route print
```
Then it is on to the app enumeration

```powershell
Get-MpComputerStatus
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```

# Inital Enumeration
[Windows Commands Reference](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)

`NT AUTHORITY\SYSTEM`: is the account or local system, having more permissions then the local Admininistrator.
`administrator` or `Administrator Group`: Is the administrator for the machine/accounts.

### Commands
```cmd
(What processes are running on the System)
tasklist /svc


(Common Information)
set
systeminfo

(Fixes/Patches)
wmic qfe

(Installed Programs)
wmic product get name

(Groups and users)
query user
echo %USERNAME%
whoami /priv
whoami /groups
net user
net localgroup
net localgroup administrators
net accounts
```

```powershell
(Running Processes)
netstat -ano


Get-HotFix | ft -AutoSize
Get-WmiObject -Class Win32_Product |  select Name, Version
```

Session Manager Subsystem (smss.exe), Client Server Runtime Subsystem (csrss.exe), WinLogon (winlogon.exe), Local Security Authority Subsystem Service (LSASS), and Service Host (svchost.exe).
