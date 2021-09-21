# Tools Cheatsheet

<!--
##################################################################
##################################################################
-->

## Hashcat

#### Show example hashes

`hashcat --example-hashes`

#### Crack .htpasswd

`hashcat -a 0 -m 1600 captured-hash.txt password-list.txt`

<!--
##################################################################
##################################################################
-->

## PDFcrack

#### Bruteforce using Wordlist

`pdfcrack -f document.pdf -w /usr/share/wordlists/rockyou.txt`

<!--
##################################################################
##################################################################
-->

## xfreerdp

#### RDP into Box

`xfreerdp hostip`

#### RDP using Username and Password

`xfreerdp -u username -p password hostip`

<!--
##################################################################
##################################################################
-->

## rdesktop

`rdesktop hostip`

<!--
##################################################################
##################################################################
-->

## [Seatbelt](https://github.com/GhostPack/Seatbelt)

**Seatbelt is a C# project that performs a number of security oriented host-survey "safety checks" relevant from both offensive and defensive security perspectives.**
**Seatbelt is a great tool for enumerating a windows host for security weaknesses.**

#### Check for UAC bypass

`Seatbelt.exe uac`

#### Show Token Privileges

`Seatbelt.exe TokenPrivileges`

<!--
##################################################################
##################################################################
-->

## [SharpUp](https://github.com/GhostPack/SharpUp)

**SharpUp can enumerate the host for any misconfiguration-based priv-esc opportunities.**

#### Run all checks

`SharpUp.exe`

#### Check for Always Install Elevated

`SharpUp.exe AlwaysInstallElevated`

#### Check for Unquoted Service Paths

`SharpUp.exe UnquotedServicePath`

<!--
##################################################################
##################################################################
-->

## [PowerView](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1)

**PowerView has long been the de-facto tool for domain enumeration.**

<!--
##################################################################
##################################################################
-->

## [MailSniper](https://github.com/dafthack/MailSniper)

**MailSniper is a penetration testing tool for searching through email in a Microsoft Exchange environment for specific terms (passwords, insider intel, network architecture information, etc.).**

#### Disable Windows Defender

`Set-MpPreference -DisableRealtimeMonitoring $true`

#### Import into Powershell

`ipmo C:\PATHTOMAILSNIPER\MailSniper\MailSniper.ps1`

#### Enumerate NetBIOS

`Invoke-DomainHarvestOWA -ExchHostname $targetip`

#### Use timing attack to validate username list

`Invoke-UsernameHarvestOWA -ExchHostname TARGETIP -Domain SOMEDOMAIN -UserList .\possible-usernames.txt -OutFile valid.txt`

#### Spray specific password against verified usernames

`Invoke-PasswordSprayOWA -ExchHostname TARGETIP -UserList .\valid.txt -Password Summer2021`

#### Get Global Address List using valid username and password

`Get-GlobalAddressList -ExchHostname TARGETIP -UserName DOMAIN\USERNAME -Password Summer2021 -OutFile gal.txt`

<!--
##################################################################
##################################################################
-->

## [SprayingToolkit](https://github.com/byt3bl33d3r/SprayingToolkit)

**A set of Python scripts/utilities that tries to make password spraying attacks against Lync/S4B & OWA a lot quicker, less painful and more efficient.**

<!--
##################################################################
##################################################################
-->

## [namemash.py](https://gist.github.com/superkojiman/11076951)

**A python script that will take a person's full name, and transform it into possible username permutations**

#### Create list

`namemash.py names.txt >> possible-usernames.txt`
