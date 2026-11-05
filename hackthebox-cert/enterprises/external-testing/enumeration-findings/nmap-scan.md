```bash
sudo nmap -sC -sV -v 10.129.243.19 -p-
21/tcp   open  ftp      vsftpd 3.0.3
|_-rw-r--r--    1 0        0              38 May 30  2022 flag.txt
22/tcp   open  ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
25/tcp   open  smtp     Postfix smtpd
|_smtp-commands: ubuntu, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
53/tcp   open  domain   (unknown banner: 1337_HTB_DNS)
| dns-nsid: 
|_  bind.version: 1337_HTB_DNS
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|     bind
|_    1337_HTB_DNS
80/tcp   open  http     Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Inlanefreight
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.41 (Ubuntu)
110/tcp  open  pop3     Dovecot pop3d
111/tcp  open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|_  100000  3,4          111/udp6  rpcbind
143/tcp  open  imap     Dovecot imapd (Ubuntu)
993/tcp  open  ssl/imap Dovecot imapd (Ubuntu)
995/tcp  open  ssl/pop3 Dovecot pop3d
8080/tcp open  http     Apache httpd 2.4.41 ((Ubuntu))
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Support Center
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
```