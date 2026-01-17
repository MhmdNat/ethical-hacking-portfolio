# Privilege Escalation
After gaining initial access through the reuse of FTP credentials in the SSH service, the focus shifted to escalating privileges in order to fully compromise the machine by abusing capabilities.

## Enumerating Binaries with Capabilities 
During capability enumeration, the following command was used to identify binaries with elevated Linux capabilities:
```bash
getcap -r / 2>/dev/null
```
`/usr/bin/python3.8` was found to have the `cap_setuid` capability enabled.

## Abusing `python3.8`'s Misconfiguration to Achieve Root
This `cap_setuid` effectively allows a process to change its `UID` to any user identity including setting it to `UID` 0 (root). By abusing this misconfiguration, a root shell can be spawned by explicitly setting the UID to 0 and executing a new shell process:
```bash
python3.8 -c 'import os; os.setuid(0); os.execl("/bin/sh", "sh");'
```
This resulted in root privileges and a full compromise of the target system.
> For a detailed explanation of `python cap_setuid` abuse techniques, see: [Linux SetUID Capabilities Abuse](/linux/privilege-escalation/setUID-capability.md)
