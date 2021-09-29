# Initial Recon Cheatsheet

<!--
##################################################################
##################################################################
-->

## Recon Tools

- [AutoRecon](https://github.com/Tib3rius/AutoRecon) **\*Automates entire recon and organization**
- [nmapAutomater](https://github.com/21y4d/nmapAutomator)

<!--
##################################################################
##################################################################
-->

## DNS Server Enumeration | port 53

#### NSLOOKUP

1. Start nslookup: <br>`nslookup`
   <br>
2. Set target DNS server as DNS server: <br> `SERVER TARGETSIP`
   <br>
3. Verify server is working: **\*Should return localhost**<br> `127.0.0.1`
   <br>
4. Point server at itself: <br> `TARGETSIP`

#### DIG

`dig axfr TARGETDOMAINNAME @TARGETDNSSERVERIP`

<!--
##################################################################
##################################################################
-->

## SMB Enumeration | port 445, 139

### rpcclient

#### Login

`rpcclient -U '' targetip`

#### Get list of users

`enumdomusers`

#### Get user info

`queryuser RID#`

#### Get info on all users

`querydispinfo`

### crackmapexec

#### Enumerate file shares

`crackmapexec smb targetip --shares`
`crackmapexec smb targetip -u '' -p '' --shares`

#### Get password policy

`crackmapexec smb targetip --pass-pol`

#### Check Login

`crackmapexec smb targetip -u username -p password`

#### Bruteforce login

`crackmapexec smb targetip -u userlist.lst -p passwordlist`

#### Crawl shares

`crackmapexec smb targetip -u username -p password -M spider_plus`

### smbclient

#### Enumerate file shares

`smbclient -L //targetip`
`smbclient -U '' -L //targetip`

### Mount share with authentication

`sudo mount -t cifs -o "user=username,password=password` //targetip/drivename /locationtomount`

<!--
##################################################################
##################################################################
-->

## SQL DB Enumeration

#### Nmap Script Scan

`nmap -Pn -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 <IP>`

<!--
##################################################################
##################################################################
-->

## LDAP | port 389, 3268

### ldapsearch

#### Get domain information

`ldapsearch -x -h targetip -s base namingcontexts`

#### Dump LDAP

`ldapsearch -x -h targetip -s sub -b 'DC=domain,DC=name'`

<!--
##################################################################
##################################################################
-->

## Windows Remote Management (winrm) | port 5985

### crackmapexec

#### Check Login

`crackmapexec winrm targetip -u username -p password`

#### Crawl shares

`crackmapexec winrm targetip -u username -p password -M spider_plus`

### evil-winrm

#### Login with valid creds

`evil-winrm -i targetip -u username -p password`
