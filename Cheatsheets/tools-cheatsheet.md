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
