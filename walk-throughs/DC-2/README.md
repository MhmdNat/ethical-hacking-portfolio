# DC-2 – Penetration Testing Case Study
This case study documents the exploitation of the DC-2 vulnerable virtual machine.
The focus of this case study is credential discovery, cross-service credential reuse, restricted shell escape, and Sudo binary misconfigurations.

---

## Key Concepts
- Content-based wordlist generation
- Wordpress credential discovery
- Cross-service credential reuse
- Restricted shell (rbash) escape
- Lateral movement between local users
- Misconfigured sudo privilege escalation

---

## High Level Attack Overview
- Identified a Wordpress application and generated a content-based wordlist
- Discovered valid Wordpress credentials through brute force
- Reused application credentials to gain SSH access
- Escaped a restricted Bash (rbash) environment
- Performed local enumeration and achieved lateral movement via credential reuse
- Escalated privileges to root through a misconfigured sudo binary
