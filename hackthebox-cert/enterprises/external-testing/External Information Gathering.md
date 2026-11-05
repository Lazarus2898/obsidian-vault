```bash
# Opening with the nmap scan
sudo nmap --open -oA inlanefreight_ept_tcp_1k -iL scope
```

# Exercise
### Question 1
Perform a banner grab of the services listening on the target host and find a non-standard service banner. Submit the name as your answer (format: word_word_word)
`sudo nmap -sC -sV -v 10.129.243.19 -p-`

### Question 2
Perform a DNS Zone Transfer against the target and find a flag. Submit the flag value as your answer (flag format: HTB{ }).
`dig axfr @10.129.243.19 inlanefreight.local`
```bash
inlanefreight.local.    86400   IN      SOA     ns1.inlanfreight.local. dnsadmin.inlanefreight.local. 21 604800 86400 2419200 86400
inlanefreight.local.    86400   IN      NS      inlanefreight.local.
inlanefreight.local.    86400   IN      A       127.0.0.1
blog.inlanefreight.local. 86400 IN      A       127.0.0.1
careers.inlanefreight.local. 86400 IN   A       127.0.0.1
dev.inlanefreight.local. 86400  IN      A       127.0.0.1
flag.inlanefreight.local. 86400 IN      TXT     "HTB{DNs_ZOn3_Tr@nsf3r}"
gitlab.inlanefreight.local. 86400 IN    A       127.0.0.1
ir.inlanefreight.local. 86400   IN      A       127.0.0.1
status.inlanefreight.local. 86400 IN    A       127.0.0.1
support.inlanefreight.local. 86400 IN   A       127.0.0.1
tracking.inlanefreight.local. 86400 IN  A       127.0.0.1
vpn.inlanefreight.local. 86400  IN      A       127.0.0.1
inlanefreight.local.    86400   IN      SOA     ns1.inlanfreight.local. dnsadmin.inlanefreight.local. 21 604800 86400 2419200 86400
```

### Question 3
What is the FQDN of the associated subdomain?
`flag.inlanefreight.local`
`flag.inlanefreight.local. 86400 IN      TXT     "HTB{DNs_ZOn3_Tr@nsf3r}"`

### Question 4
Perform vhost discovery. What additional vhost exists? (one word)
`ffuf -w /usr/share/seclists:FUZZ -u http://domain.com:port/ -H 'Host:FUZZ.domain.com'`
```bash
blog                    [Status: 200, Size: 8708, Words: 1509, Lines: 232, Duration: 476ms]
careers                 [Status: 200, Size: 51806, Words: 22041, Lines: 732, Duration: 95ms]
dev                     [Status: 200, Size: 2048, Words: 643, Lines: 74, Duration: 93ms]
gitlab                  [Status: 302, Size: 113, Words: 5, Lines: 1, Duration: 126ms]
ir                      [Status: 200, Size: 28548, Words: 2885, Lines: 210, Duration: 1308ms]
monitoring              [Status: 200, Size: 56, Words: 3, Lines: 4, Duration: 93ms]
```
`monitoring`