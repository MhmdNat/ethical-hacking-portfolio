# Kioptrix‑3 - VulnHub - Penetration Testing Case Study
This case study documents the exploitation of the Kioptrix‑3 virtual machine from VulnHub. It focuses on web application misconfigurations, `SQL` injection, cross‑service credential reuse, and `sudo` misconfiguration.

## Key Concepts
- `SQL` Injection (SQLi)
- Credential Exposure via Database Compromise
- Cross‑Service Credential Reuse
- `Sudo` Misconfiguration

## High‑Level Attack Overview
**1. Initial Enumeration**

Web application enumeration revealed a vulnerable parameter within the `gallery` functionality that was directly reflected in backend database queries without proper sanitization.

**2. SQL Injection Exploitation**

The identified injection point was confirmed and further exploited using automated techniques. Through SQL injection, backend databases were enumerated, leading to the discovery of a database containing developer account information. Credential data, including hashed passwords, was extracted from the database and successfully cracked, exposing valid developer credentials.

**3. Initial Access**

Using the cracked credentials, SSH access was obtained on the target system. While one account was constrained by a restricted shell, another provided a standard interactive shell and was used as the primary foothold.

**4. Privilege Escalation**
Local enumeration revealed that the compromised user was permitted to execute the `/usr/local/bin/ht` binary with `root` privileges without password authentication via `sudo`. Since `ht` is a text editor capable of modifying system files, this misconfiguration allowed controlled modification of privileged configuration files. By abusing this, privileges were escalated to `root`, resulting in full system compromise.
