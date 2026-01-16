# Privilege Escalation Method - Sudo Misconfiguration Abuse

## Vulnerability
The vulnerability stems from the misconfiguration that allows spawning a shell with elevated privileges

## Conditions
- A binary with sudo privileges for another user is accessible to the compromised user 
- The access to the binary is either passwordless, or the pasword for the user is known

## Exploitation and Key Commands
Enumerate sudo privileges using
```bash
sudo -l
```
Identify exploitable binaries (e.g. via GTFOBins)

## Examples
Below are examples from different binaries, content adopted from [GTFOBins](https://gtfobins.github.io/)

### `git` 
When executed with elevated privileges, `git` can invoke external pagers (such as `less`) to display help pages, or logs. These pagers support shell escape functionality, which can be abused to spawn an interactive shell inheriting the privileges of the parent `git` process.

By triggering a pager while `git` is executed via `sudo`, arbitrary command execution as the privileged user becomes possible.

Example exploitation:
```bash
sudo git branch --help config
!/bin/sh
```
---

