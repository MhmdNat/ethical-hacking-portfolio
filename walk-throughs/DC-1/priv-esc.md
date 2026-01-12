# Privilege Escalation of DC-1 virtual machine
After gaining initial access to the machine as the low-privilege web user, further privilege escalation was required in order to fully compromise the system

## General
Local enumeration was first performed to identify potential escalation paths
key findings include:
  - The system was running Linux kernel 3.2, which is known to have public privilege escalation exploits. However, kernel exploitation was deprioritized in favor of misconfigurations such as SUID binaries, as kernel exploits are generally less stable and less representative of real-world attack paths.
  - The `www-data` user had no sudo privileges.

## SUID Binaries
- I then went to look for binaries owned by root with SUID, which would allow execution of said binaries with root privileges
  ```bash
  find / -perm -4000 -type f 2>/dev/null
  ```
  which resulted in ```-rwsr-xr-x 1 root root ... /usr/bin/find``` indicating that the find binary has SUID
- Using `find` an interactive shell was spawned by utilizing the `exec` option which allows for code execution as root since `find` is a binary owned by root
  ```bash
  find . -exec /bin/sh \;
  ```
  Note: On this system, `/bin/sh` did not drop elevated privileges, allowing the shell to inherit the effective UID of the SUID `find` binary. On newer systems, -p might be required to preserve privileges, but it was not supported on this machine
- This resulted in successful privilege escalation to the root user. </br>
```bash
id
whoami
```
  <img width="296" height="75" alt="image" src="https://github.com/user-attachments/assets/a06f7836-4ae0-4f98-85af-63ff0113dc28" />
