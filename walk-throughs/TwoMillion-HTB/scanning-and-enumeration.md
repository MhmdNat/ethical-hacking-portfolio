# Scanning and Enumeration of TwoMillion VM
This phase focuses on identifying exposed services and enumerating the web application to find possible attack vectors

## Initial Scanning of Exposed Services
An initial service scan was conducted to identify all open `TCP` ports of the target machine and possible running services:
```bash
nmap -Pn -p- <two-million ip>
```
A more targeted service-version scan was conducted on open ports to identify possible vulnerable versions:
```bash
nmap -Pn -sV -p 22,80 <two-million ip>
```

The scan revealed two main services:
- 22/tcp - `SSH` (OpenSSH 8.9p1)
- 80/tcp - `HTTP` (nginx)

Between these two services, the web application proved to be most interesting, as no initial attack vectors were identified on `SSH`. This led to further enumeration of the web application.


## Web Application Enumeration
On the main page of the web app, the only endpoint found was the login page. As functionality was limited on the website, enumeration focused on this page.

### Login Page
The login page required valid credentials and did not allow account creation without first obtaining a valid invitation code. This indicated that the invitation generation logic must exist somewhere within the application.

Rather than attempting immediate authentication attacks such as SQL Injection, the focus shifted toward discovering how invitation codes were generated.

### Invitation Code Page
The invitation page contained only one input field where users can submit a valid invitation code to proceed with registration.

Since the application relied on client-side functionality to interact with `API` functionalities, the browser Developer Tools Network tab was used to inspect requests made by the page. During this inspection, an interesting JavaScript file was identified: 
```
/js/inviteapi.min.js
```
Because the filename suggests interaction with an API responsible for invitation functionality, this file became a key target for further analysis.

## Invite API `JavaScript` File
Upon inspection, the file appears to be obfuscated. The obfuscation method was recognized as *Dean Edwards' Packer* and was identifiable by its signature parameters:
```
(p, a, c, k, e, d)
``` 
This form of obfuscation is commonly used to compress or hide JavaScript logic while still allowing it to execute in the browser.
To analyze the script, the `eval()` call responsible for executing the packed code was replaced with `console.log()`. This allowed the decoded JavaScript to be printed directly in the browser console.

After deobfuscation, two functions were recovered:
- `verifyInviteCode()`: which calls `/api/v1/invite/verify` to verify whether the invitation code is valid or not.
- `makeInviteCode()`: which calls `/api/v1/invite/how/to/generate`. The presence of this function is significant because it exposes internal API functionality related to invitation code generation. Ideally, such endpoints should be restricted or removed from client-side code to prevent unauthenticated users from discovering backend functionality.

Further investigation of these API endpoints ultimately led to the initial exploitation of the system, which is covered in the exploitation phase.
