# Nmap Cheatsheet

## My Go-to scan
`nmap -sV -sC -vvv -oN scan.txt 10.10.10.198`

## Full TCP port scan using service version detection
`nmap -p 1-65535 -sV -sS -T4 target`

## Firewall Bypass

#### Send scans from spoofed IPs.
`nmap -D RND:10`

#### Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters.
`nmap -f`

#### Use given source port number.
`nmap -g 53 *usually 20,53 and 67`

#### Send Bad Checksums
`nmap --badsum`

#### Firewall Bypass script
`nmap --script firewall-bypass`
