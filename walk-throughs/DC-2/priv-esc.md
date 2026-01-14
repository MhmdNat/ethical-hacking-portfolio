# Privilege Escalation in DC-2 Virtual Machine
After initial access to the machine as a user with a restricted shell, further escalation was required in order to fully compromise the system

## Escaping `rbash`
Enumeration of the `$PATH` variable revealed four binaries that could be executed through this restricted shell:
  - ls
  - less
  - vi
  - scp
  Out of these four, vi was chosen as the main attack vector for its ability to spawn subprocesses
---
- The initial attemp to execute `:! /bin/sh` in vi failed as the `/` character is restricted
- However, after changing the internal shell of vi to `/bin/sh` and invoking it through `:shell`, an interactive shell was spawned, successfully escaping rbash

## Lateral Movement through Jerry
- After successfully escaping rbash, the third flag indicated that we should switch to user `jerry`
- Lateral movement was achieved because of the reuse of credentials, which allowed unauthorized access into the `jerry` user through `su jerry`
- The fourth flag was found, hinting towards the `git` binary
- After listing sudo permissions using `sudo -l` it was noted that root access was allowed for the `git` binary, without any authentication, which will be used to escalate privileges

## Privilege Escalation through `git`
