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
