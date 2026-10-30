# Starting Point
When landing into an environment, getting a lay of the land is key
```bash
whoami
id
hostname
ifconfig or ip a
sudo -l
sudo -V
sudo su
uname -a
ps aux | grep root (By user)
lscpu
lsblk
lastlog
w
df -h
lpstat (To find any printers)
getent group sudo (lists members of interesting groups)
```
# Finding Files
```bash
# Hidden Files
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student

# Hidden Directories
find / -type d -name ".*" -ls 2>/dev/null

# Hidden History
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null

# Processes
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"

# Services
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list

# Config Files
find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null

# Scripts
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
```
# File/Other methods
```bash
cat /etc/os-release
cat /etc/passwd
cat /etc/passwd | cut -f1 -d: (Finding the users)
cat /etc/group
cat /etc/shadow
cat /etc/shells
ls -la /etc/cron.daily/
cat /etc/fstab
cat /etc/fstab | grep -v "#" | column -t (Unmounted filing systems)
ls -l /bin /usr/bin/ /usr/sbin/ (Binaries)
ls -l /tmp /var/tmp /dev/shm (Temp files)
cat /etc/resolv.conf
cat ~/.bashrc
cat ~/.bash_history
echo $PATH
env
route
arp -a
strace ping -c1 10.129.112.20 (Trace system calls)
```

# GTFO Bins
```bash
for i in $(curl -s https://gtfobins.org/api.json | jq -r '.executables | keys[]'); do if grep -q "$i" installed_pkgs.list; then echo "Check for GTFO: $i";fi; done
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