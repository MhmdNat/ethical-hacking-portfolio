# Initial Access Method - NFS SSH Key Injection

## Vulnerability 
The vulnerability stems from a NFS write access to `~/.ssh/authorized_keys` enabling SSH authentication injection.

## Conditions
- NFS service reachable
- Write access to the target user’s `~/.ssh` directory over NFS
- SSH authentication using private-public-key pair enabled

## Exploitation and Key Commands
The following assumes the attacker has a mount point on /mnt/peter:

- Change permissions because OpenSSH rejects keys if:
  - `~/.ssh` is group/world writable
  - authorized_keys is too permissive
```bash
chmod 700 /mnt/peter/.ssh && chmod 600 /mnt/peter/.ssh/authorized_keys
```
- Add public key generated from `ssh-keygen` saved in /home/user/.ssh/id_ed25519.pub, into the 'authorized_keys' file.
- SSH into machine using `ssh -i <path to private key on attacker machine> <victim's user>@<victim's ip>`
