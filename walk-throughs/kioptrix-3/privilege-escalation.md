## Privilege Escelation in Kioptrix-3 VM
After gaining an initial foothold on the `loneferret` user through the exposed `SSH` service, privilege escalation was performed using `sudo` misconfigurations.


## Local Enumeration
Enumeration of sudo privileges using:
```bash
sudo -l
```
revealed that the `loneferret` user is permitted to execute the binary `/usr/local/bin/ht` as the root user without requiring a password.
The ht binary is a text editor. When executed with root privileges, it allows modification of system files owned by root, presenting a clear privilege escalation opportunity.


## Abusing Sudo Misconfiguration
The ht editor was executed with elevated privileges:
```bash
sudo /usr/local/bin/ht
```
Using this editor, the /etc/sudoers file was modified to grant the loneferret user full sudo privileges. Specifically, an entry was added allowing the user to execute commands as root without password authentication.
After updating the sudoers configuration, a root shell was obtained by executing:
```bash
sudo /bin/bash
```
Successful escalation was confirmed by verifying the effective user ID:
```bash
id
```
which showed the session running as the root user.
