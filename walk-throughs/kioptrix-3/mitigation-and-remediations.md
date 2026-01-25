# Mitigations and Recommendations
The following remediation steps are recommended to mitigate the vulnerabilities identified during the assessment.

## Remove Exposed and Unused Application Endpoints
Commented or unused application endpoints should be removed from production source code. Leaving administrative paths such as `/gadmin` referenced in `HTML` comments exposes internal application structure and assists attackers during enumeration. Only functionality intended for end users should be present in exposed source code.


## Enforce Server-Side Authorization and Input Validation
Unauthorized users were able to communicate with the `gallery` delete endpoint

**Recommendations:**
- Verify user authentication and authorization
- Enforce role-based access controls
- Reject unauthorized requests regardless of how they are constructed
- This prevents unauthorized users from interacting with privileged endpoints even if they are discovered


## Mitigate SQL Injection Vulnerabilities
The SQL injection vulnerability was caused by user-controlled input being directly incorporated into SQL queries.

**Recommendations:**
- Use parameterized queries / prepared statements
- Avoid dynamic SQL query construction
- Run the database using a least-privileged database account


## Prevent Credential Reuse Across Services
Credentials obtained from the web application were successfully reused to authenticate to the `SSH` service, providing an initial foothold.

**Recommendations:**
- Enforce unique credentials per service
- Restrict `SSH` access to necessary users only
- Consider disabling password-based `SSH` authentication in favor of key-based authentication
- Restrict `SSH` access to specific `IP`s through the firewall


## Correct Sudo Misconfigurations and Apply Least Privilege
The loneferret user was allowed to execute a text editor (`ht`) as `root` without authentication, enabling arbitrary file modification and privilege escalation.

**Recommendations:**
- Remove unnecessary sudo permissions
- Do not allow interactive editors to run as `root` via sudo
- Apply the principle of least privilege to all `sudo` rules
- Regularly audit `sudo` configurations for misuse potential
