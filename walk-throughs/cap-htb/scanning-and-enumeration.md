# Scanning and Enumeration
This phase focused on identifying exposed network services and assessing the web application as the primary attack surface.

## Identifying Exposed Services using `nmap`
An initial service and version scan was performed to identify exposed services and determine potential entry points.
```bash
nmap -sV <cap ip>
```
The following services were identified:
- 21/tcp - ftp (vsftpd 3.0.3)
- 22/tcp SSH (OpenSSH 8.2)
- 80/tcp HTTP running on Gunicorn
No direct attack vectors or misconfigurations that allow authentication bypass through FTP or SSH were observed at this preliminary stage. Thus, the web application was the most promising entry point and required further enumeration.

## Manual Enumeration of the Web Application
During web enumeration, the `Security Snapshot` tab associated with the user `Nathan` was found. further inspection revealed an endpoint with a user-controlled numeric identifier `/data/1`. Decrementing the identifier to `/data/0`, allowed viewing another user's data. Confirming the existence of an Insecure Direct Object Reference and effectively giving us access to the sniffed packets.

