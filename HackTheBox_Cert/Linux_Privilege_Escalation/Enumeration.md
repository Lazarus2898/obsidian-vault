# Starting Point
When landing into an environment, getting a lay of the land is key
```bash
whoami
id
hostname
ifconfig or ip a
sudo -l
sudo su
uname -a
lscpu
lsblk
df -h
lpstat (To find any printers)
getent group sudo (lists members of interesting groups)
```

# Other methods
```bash
cat /etc/os-release
cat /etc/passwd
cat /etc/passwd | cut -f1 -d: (Finding the users)
cat /etc/group
cat /etc/shadow
cat /etc/shells
cat /etc/fstab
cat /etc/fstab | grep -v "#" | column -t (Unmounted filing systems)
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student (Hidden files)
find / -type d -name ".*" -ls 2>/dev/null (Hidden Dirs)
ls -l /tmp /var/tmp /dev/shm (Temp files)
cat /etc/resolv.conf
cat ~/.bashrc
cat ~/.bash_history
echo $PATH
env
route
arp -a
```

Things to lookout for:
- [Exec Shield](https://en.wikipedia.org/wiki/Exec_Shield)
- [iptables](https://linux.die.net/man/8/iptables)
- [AppArmor](https://apparmor.net/)
- [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
- [Fail2ban](https://github.com/fail2ban/fail2ban)
- [Snort](https://www.snort.org/faq/what-is-snort)
- [Uncomplicated Firewall (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall)

# Hashes
|**Algorithm**|**Hash**|
|---|---|
|Salted MD5|`$1$`...|
|SHA-256|`$5$`...|
|SHA-512|`$6$`...|
|BCrypt|`$2a$`...|
|Scrypt|`$7$`...|
|Argon2|`$argon2i$`...|