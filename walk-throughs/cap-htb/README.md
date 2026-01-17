# Cap - Hack The Box - Penetration Testing Case Study
This case study documents the exploitation of the Hack The Box Cap machine. The case study focuses on broken access control, insecure credential handling, credential reuse, and Linux capability misconfiguration.

---

## Key Concepts
- Insecure Direct Object Reference (`IDOR`)
- Cleartext Credential Exposure
- Cross-Service Credential Reuse
- Linux Capabilities Abuse

---

## High-Level Attack Overview

**1. Initial Enumeration**
  - A numeric identifier parameter was identified as user-controlled.

**2. IDOR Exploitation**
  - By modifying the numeric identifier, sensitive data belonging to other users was accessed.
  - `FTP` credentials were retrieved through this unauthorized access.
    
**3. Initial Access**
  - The extracted `FTP` credentials were reused to authenticate to the `SSH` service.
  - An initial foothold on the target system was gained through a low-privileged shell.
    
**4. Privilege Escalation**
  - Linux capability enumeration revealed that `/usr/bin/python3.8` had the `cap_setuid` capability enabled.
  - This misconfiguration allowed escalation to `UID 0` by abusing Python’s ability to change its effective user ID.

A root shell was obtained, fully compromising the system.
