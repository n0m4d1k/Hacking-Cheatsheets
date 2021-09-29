# OSINT

### Email Server Enumeration

#### Show Sender Policy Framework (SPF) Record

`dig +short TXT targetdomain`

#### Check DomainKeys Identified Mail (DKIM) Record

`dig dkim._domainkey.targetdomain TXT`

#### Check if domain uses DMARC

`dig +short TXT _dmarc.targetdomain`
