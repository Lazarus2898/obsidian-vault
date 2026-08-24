# The Base Information
```bash
Port 80 = HTTP
Port 443 = HTTPS
```

### Tools Used
```bash
Burpsuite
OWASP Zed Attack Proxy (ZAP)
```

##### Burpsuite
```bash
Proxy->Intercept

# Default is set to 8080
Under Proxy->Proxy, Settings->Proxy
# Configuring the Proxy
Using Foxy Proxy (Links at the bottom)
Going to options
Click add, use 127.0.0.1:8080, then name it Burp
Go to http://Burp to download the Cert.
Viewing 'about:preferences#privacy' and clicking on 'View Certificates'.
Go to authorities and click the import tab to use the certificate
Finally selecting the 'Trust this CA to identify Websites and email users.'
```

##### Results
```bash
# By changing the request information, you are able to modify the possible output.
```
![[Pasted image 20260806105816.png]]
##### Automatic Request Modification
```Bash
# Going to Proxy->Proxy Settings->HTTP match and replace rules
# Adding rules
{
	"key:1":"value2"
}
```

`Type`: `Request header` - Since the change we want to make will be in the request header and not in its body.
`Match`: `User-Agent:` - The regex pattern that matches the entire line with User-Agent in it.
`Regex match: True` - We don't know the exact User-Agent string we want to replace, so we'll use regex to match any value that matches the pattern we specified above.

##### Repeater
```bash
# Copying and sending the Post request to repeater you can run commands faster.
# In the example above finding the flag with.
ip=; pwd; cd ../../..; ls; flag.txt;
```

##### Encoding & Decoding the URL
Going to the burp inspector.
`Encoding`
`Spaces` = End of request data if not encoded
`&` = Otherwise interpreted as a parameter delimiter
`#` = Otherwise interpreted as a fragment identifier
`Convert Selection->URL->URL-encode key characters`

`Decoding`
`HTML, Unicode, Base64, and ASCII hex`.


As we can see, the text holds the value `{"username":"guest", "is_admin":false}`. So, if we were performing a penetration test on a web application and find that the cookie holds this value, we may want to test modifying it to see whether it changes our user privileges. So, we can copy the above value, change `guest` to `admin` and `false` to `true`, and try to encode it again using its original encoding method (`base64`):

Putting the string through the decoder, using base64 then URL changed the output to the flag.

##### Proxying Tools
`/etc/proxychains`
`http 127.0.0.1 8080`
Then curling a site with `proxychains -q curl http://IP:port`
You will be able to see it on burpsuite.

Now going through `msfconsole`
```bash
use auxiliary/scanner/http/robots_txt
set PROXIES HTTP:127.0.0.1:8080
SET RHOST IP
SET RPORT PORT
run
```
You will see on burp that specific page.
# Links
[FoxyProxy Standard – Get this Extension for 🦊 Firefox (en-US)](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/)

[SecLists](https://github.com/danielmiessler/SecLists.git)