# Mitigation and Recommendations
To reduce both the likelihood of initial compromise and the impact of post-exploitation, the following mitigations are recommended:

## Update the operating system and kernel
The system was running an outdated Linux kernel (3.2) with publicly known privilege escalation vulnerabilities. Updating the kernel to a supported and fully patched version significantly reduces exposure to kernel-level exploits.

## SUID permissions 
- The find binary should not be SUID-enabled under normal circumstances, as it allows arbitrary command execution when misconfigured. The SUID bit should be removed unless there is a strict operational requirement:
```bash
chmod u-s /bin/find
```
- Other recommendations include:
  - Audit SUID and SGID binaries regularly to identify unnecessary or misconfigured binaries
  - Ensure only essential binaries retain elevated privileges
  - Apply the principle of least privilege to services and users

## CMS related recommendations
- Keep web applications and dependencies up to date
- Vulnerabilities such as Drupalgeddon demonstrate how outdated CMS software can lead to remote code execution and initial system compromise. Timely patching and version management are critical at the application layer.
- Run web services under restriced service users to minimize exploitation impact
