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

## Examples
Below are examples from different binaries, _content adopted from [GTFOBins](https://gtfobins.github.io/)_

### `find`

The `find` binary has an `-exec` option that allows execution of commands
When SUID-enabled and the binary is owned by `root`, this can be abused to spawn an interactive shell

```bash
find . -exec /bin/sh \;
```
If the invoked shell does not drop elevated privileges, the attacker obtains a shell with an effective UID of 0 (root).
---
> _*Note:*_ On newer systems, shells may drop privileges by default or require additional flags (`/bin/sh -p`).
Behavior varies depending on system configuration and binary versions.

