

# MYSQL

```powershell

# Enum
sudo nmap $ip -Pn -n -sV -sC -p3306 --script mysql* > mysqlScan.log

# unathurized  access
mysql -u root -h $ip

# Autorized access
mysql -u root -p'root' -h 192.168.208.16 -P 3306
# If ERROR 2026 (HY000) TLS/SSL error shows up, we can append --skip-ssl at the end of the command to connect to the database.
mysql -u root -p'root' -h 192.168.208.16 -P 3306 --skip-ssl
# NOTE: there must be no space between -p and the 'root' password: -p'root'

# SQL Commands:
show databases
select version()
use <database>
show  tables
select * from <table>;
show columns from <table>;
select * from <table> where <column> = "<string>";



# Get the version and do vulnerability assessment
# MySQL accepts both _version()_ and _@@version_ statements.
select version();

# The current database user
select system_user();

# Show databases;
show databases;

# select a database
use <database name>;

# show tables (after selecting a db)
show tables;

# You can view the columns of the mysql.user table using either DESCRIBE or SHOW COLUMNS.
DESCRIBE mysql.user;
SHOW COLUMNS FROM mysql.user;

# Retrieve the password of the offsec user present in the mysql database.
SELECT user, authentication_string FROM mysql.user WHERE user = 'offsec';

# Retrieve the plugin value of the offsec user present in the mysql database.
SELECT user, plugin FROM mysql.user WHERE user = 'offsec';

```


