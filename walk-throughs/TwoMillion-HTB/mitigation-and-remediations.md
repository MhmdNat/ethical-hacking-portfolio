#Mitigations and Recommendations
The following remediation steps are recommended to mitigate the vulnerabilities identified during the assessment of the TwoMillion HTB VM.

## API Security and Broken Access Control
The assessment revealed multiple API endpoints that lacked proper access control. This allowed a non-privileged user to escalate privileges by manipulating client-supplied parameters (is_admin) and access administrative functionality.

**Recommendations:**
- Enforce proper authorization checks on all API endpoints, especially administrative and sensitive functionality.
- Never rely on client-supplied data to determine privilege levels. Validate all parameters server-side.
- Implement role-based access control (RBAC) with strict separation between user and admin endpoints.
- Log and monitor unauthorized access attempts to detect abuse.


## Input Validation and Command Injection
Administrative endpoints, such as VPN generation, accepted user-controlled input that was passed directly to system commands without sanitization. This allowed command injection and remote code execution.

**Recommendations:**
- Never pass user input directly to system commands. Use safe libraries or API calls instead.
- Validate and sanitize all input to prevent injection attacks (;, |, &, etc.).
- Use a least-privilege system account for any service performing system operations, not the web server user.
- Consider running system-level operations in a sandboxed environment or container.

##Credential Management and Reuse
Credentials for the application were reused across system accounts, allowing lateral movement from a web shell to SSH access as admin.

**Recommendations:**
- Enforce unique, strong passwords for each service or system account.
- Do not store sensitive credentials in web-accessible directories or files.
- Consider implementing multi-factor authentication (MFA) for sensitive accounts.
- Regularly audit for credential reuse across services.
- Harden authentication with PAM (Pluggable Authentication Modules): enforce password complexity, expiration policies, and account lockouts after failed login attempts.

## Kernel and OS Hardening
The system was running an outdated Linux kernel vulnerable to `CVE-2023-0386`, which allowed local privilege escalation to root.

**Recommendations:**
- Keep the operating system and kernel fully up to date with security patches.
- Subscribe to security advisories for deployed kernel versions.
