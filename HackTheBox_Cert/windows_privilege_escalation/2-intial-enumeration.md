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
