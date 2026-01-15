# Mitigation and Recommendations
To reduce both the likelihood of initial compromise and the impact of post-exploitation, the following mitigations are recommended

## Add Password Policy
Enforce a system-wide password policy using Pluggable Authentication Modules - PAM (e.g., pam_pwquality) and /etc/login.defs. This reduces effectiveness of password guessing by enforcing password minimum length and complexity constraints.

## Prevent Cross-Service Credential Reuse
Passwords used for web applications should not be used for system-level authentication. This seperation of web application credentials and system credentials reduces the risk of lateral access accross services.

Mitigations include:
  - Restricting SSH authentication to dedicated system users not used by applications
  - Disabling password-based SSH authentication in favor of public-private key authentication
  - Restrict SSH access to certain IPs through firewall configurations or IP allowlists
 
## Restrict Sudo Privileges
Sudo permissions should follow the principle of least privilege. Allowing the execution of complex binaries such as git with root privileges introduces indirect shell escape vectors.

Mitigations include:
  - Avoiding unrestricted sudo permissions
  - Restricting sudo access to purpose-built administrative commands
  - Regular auditing of sudoers configuration
