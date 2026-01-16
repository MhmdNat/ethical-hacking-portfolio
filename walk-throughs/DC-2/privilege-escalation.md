# Privilege Escalation in DC-2 Virtual Machine
After initial access to the machine as a user with a restricted shell, further escalation was required in order to fully compromise the system

## Escaping `rbash`
Enumeration of the `$PATH` variable revealed four binaries that could be executed through this restricted shell including `ls`, `less`, `vi`, `scp`. Out of these four, vi was chosen as the main attack vector for its ability to spawn subprocesses. 

The initial attempt to execute `:! /bin/sh` in vi failed as absolute paths are restricted.  However, since vi allows the configuration of its internal shell handler, changing the internal shell to `/bin/sh`  with `:set shell=/bin/sh` and invoking it through `:shell`, an interactive shell was spawned.

This bypass was successfull because restrictions imposed on rbash do not propagate to sub-processess spawned by allowed binaries.

## Lateral Movement through Jerry
After escaping the restricted shell, further enumeration revealed credential reuse associated with the jerry user. Due to credential reuse across services, these credentials were successfully used with `su jerry`, enabling lateral movement to a higher-privileged local user.

## Privilege Escalation through `git`
Enumeration of sudo privileges using `sudo -l` revealed that the jerry user was permitted to execute the `git` binary as passwordless root. Since git invokes external pagers that allow shell spawns, this misconfiguration makes it possible to spawn shells with elevated privileges and thus full compromise of the system was achieved.
> For a detailed explanation of `git` privilege abuse techniques, see: [Linux Sudo Privilege Abuse](/linux/misconfigured-sudo-binaries.md)
