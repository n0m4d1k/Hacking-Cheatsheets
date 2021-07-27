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

## DNS Server Enumeration Port 53

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

## SMB Enumeration

####

<!--
##################################################################
##################################################################
-->
