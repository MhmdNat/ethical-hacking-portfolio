# Mitigation and Recommendations 
To reduce the chance of initial compromise through the `IDOR` vulnerability and cross-service credential reuse, and prevent privilege escalation through capability abuse, the following mitigations are recommended.


## Prevent Insecure Direct Object Reference (`IDOR`)
The application relied on a user-controlled numeric identifier to retrieve user data. By modifying this identifier, it was possible to access data belonging to other users without authorization.

**Recommendations:**
- Do not trust user-supplied identifiers to control access to data.
- Ensure the server verifies that the requested data belongs to the authenticated user before returning it.
- Reject requests where a user attempts to access data that does not belong to them.


## Secure Credential Transmission over `FTP` 
`FTP` credentials were transmitted in plaintext, allowing them to be intercepted.

**Recommendations:**
- Replace `FTP` with a secure alternative such as:
  - `FTPS` (`FTP` over `TLS`) to encrypt credentials and data in transit.
  - `SFTP` (`SSH` File Transfer Protocol), which provides encryption by default.


## Prevent Cross-Service Credential Reuse
The extracted `FTP` credentials were reused and authenticated through `SSH` which allowed unauthorized access.

**Recommendations:**
- Use service-specific system accounts for services such as `FTP` instead of sharing credentials across services
- Disable password-based `SSH` authentication in favor of public-private key authentication
- Restrict `SSH` access to certain IPs through firewall configurations or IP allowlists


## Remove Dangerous Linux Capabilities from Interpreters
The binary `/usr/bin/python3.8` was assigned the `cap_setuid` capability, allowing any user who could execute it to escalate privileges by setting their `UID` to root.

**Recommendations:**
- If the Python binary is required to bind to privileged ports, only the `cap_setuid` capability should be removed while retaining `cap_net_bind_service`:
```bash
setcap 'cap_setuid-ep' /usr/bin/python3.8
```
- Regularly audit system binaries for dangerous capabilities
- Apply the principle of least privilege: services and binaries should only have the minimum permissions required to function
