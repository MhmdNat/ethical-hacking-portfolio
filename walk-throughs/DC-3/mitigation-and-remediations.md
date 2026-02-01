# Mitigations and Recommendations
The following remediation steps are recommended to mitigate the vulnerabilities identified during the assessment.


## Joomla Hardening
Initial access was achieved through a `SQL` injection vulnerability in a Joomla component, allowing unauthorized access to the database and disclosure of sensitive information.

**Recommendations:**
- Patch Joomla core and third-party components to supported versions
- Remove or disable unused and vulnerable extensions
- Validate and sanitize all user-controlled input server-side
- Use prepared statements and parameterized queries


## Database Security
The Joomla application was configured to use a high-privilege database account, and database credentials were exposed through an application configuration file. This significantly increased impact after initial compromise.

**Recommendations:**
- Avoid using the database root account for web applications
- Create a dedicated database user with only required permissions
- Store database credentials securely outside of web-accessible files
- Restrict read permissions on configuration files from the web server user


## Kernel Hardening
The target system was running an outdated Linux kernel vulnerable to known local privilege escalation exploits, allowing an attacker to escalate privileges after gaining local access.

**Recommendations:**
- Keep the operating system and kernel fully up to date
- Monitor for publicly disclosed kernel vulnerabilities affecting deployed systems
