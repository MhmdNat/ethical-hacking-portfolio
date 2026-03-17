# TwoMillion HTB - Penetration Testing Case Study
This case study documents the exploitation of the TwoMillion virtual machine from Hack The Box. The assessment focuses on API enumeration, broken access control, command injection, credential exposure, and local privilege escalation using a Linux kernel vulnerability.


##Key Concepts
- API Enumeration and Hidden Functionality Discovery
- Broken Access Control and Privilege Escalation
- Command Injection via Administrative Endpoints
- Credential Exposure and Reuse
- Kernel-Based Local Privilege Escalation (`CVE-2023-0386`)


## High-Level Attack Overview
**1. Initial Enumeration**
- Manual inspection and analysis of the web application revealed internal `JavaScript` logic containing a `makeInviteCode()` function.
- This led to discovery of hidden API endpoints for invite code generation and user management.
- Unauthenticated access to the `/api/v1/invite/how/to/generate` endpoint exposed internal logic using weak `ROT13`encoding rather than proper authorization checks.


**2. Invite Code Generation and User Registration**
- The `ROT13`-encoded instructions were decoded, revealing the endpoint for generating invite codes.
- A request to `/api/v1/invite/generate` produced a `Base64`-encoded code, which was decoded to register a standard user account.
- Gaining authenticated access allowed enumeration of additional endpoints, including VPN management and administrative functionality.

 
**3. Broken Access Control**
- The endpoint `/api/v1/admin/settings/update` did not enforce proper authorization, trusting client-supplied parameters for privilege escalation.
- By setting `"is_admin": 1` for the authenticated user, administrative privileges were gained.


**4. Command Injection**
- Administrative access enabled testing of `/api/v1/admin/vpn/generate`, which invoked backend scripts to generate VPN configuration files.
- Input validation was insufficient, allowing command injection via the username parameter.
- Initial tests using `sleep 5` confirmed blind command execution.
- A reverse shell payload was executed, establishing a remote connection as the `www-data` user.


**5. Credential Discovery and Reuse**
- Local enumeration revealed a `.env` file containing database credentials.
- Attempting to use these credentials against local system accounts revealed password reuse, allowing `SSH` access as admin.


**6. Privilege Escalation (`CVE-2023-0386`)**
- The admin user’s mailbox contained a hint about `OverlayFS/FUSE` kernel vulnerabilities.
- `CVE-2023-0386` was identified as a local privilege escalation vector affecting the Linux kernel.
- Exploit code was transferred, compiled, and executed, yielding a root shell.
- Root access confirmed full system compromise.
