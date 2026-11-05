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
tasklist /svc /FI "PID eq <PID>"


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
net user <user>
net localgroup
net localgroup administrators
net accounts
```

```powershell
(Running Processes)
netstat -ano
netstat -ano | findstr :8080



Get-HotFix | ft -AutoSize
Get-WmiObject -Class Win32_Product |  select Name, Version
```

Session Manager Subsystem (smss.exe), Client Server Runtime Subsystem (csrss.exe), WinLogon (winlogon.exe), Local Security Authority Subsystem Service (LSASS), and Service Host (svchost.exe).

# Communication with Processes
`netstat -ano`
The main thing to look for with Active Network Connections are entries listening on loopback addresses (127.0.0.1 and ::1) that are not listening on the IP Address (10.129.43.8) or broadcast (0.0.0.0, ::/0). 

### Named Pipes
[Named Pipes](https://docs.microsoft.com/en-us/windows/win32/ipc/named-pipes)

```cmd
pipelist.exe /accepteula

(Also using Accesschk can enumerate pipes)
accesschk.exe /accepteula \\.\Pipe\lsass -v
```
One can even perform attacks with said pipe tools
[Windscribe Name Pipe Priv Escal](https://www.exploit-db.com/exploits/48021)

`accesschk.exe -accepteula -w \pipe\WindscribeService -v`

Additionally, can list the pipes in Powershell
```powershell
gci \\.\pipe\
```

To find the right pipes for the exercise 
`Get-CimInstance Win32_Service | Where-Object {$_.Name -eq 'MSSQL$SQLEXPRESS01'} | Format-List *`