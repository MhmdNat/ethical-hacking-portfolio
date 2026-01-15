# DC-1: Linux Privilege Escalation Case Study

This repository documents the exploitation of the **DC-1** vulnerable Linux virtual machine.  
The focus of this case study is **enumeration and SUID binary abuse**, along with realistic mitigation recommendations.

---

## Scope
- Performed local enumeration after initial access
- Identified privilege escalation vectors on Linux
- Exploited misconfigured SUID binaries
- Understood the risks of outdated CMS software (e.g., Drupal)
- Documented exploitation and mitigation in a pentest style

---

## High-Level Attack Overview
1. Initial access obtained through a vulnerable web application (Drupal-based CMS)
2. Local enumeration performed as a low-privilege web user
3. Discovery of a misconfigured SUID binary (`find`) owned by root
4. Privilege escalation achieved via SUID abuse
5. Root access confirmed
6. Mitigation strategies proposed to prevent recurrence

---

## Repository Structure
DC-2/
├── README.md                         # Overview and attack summary
├── scanning-and-enumeration.md       # Service discovery and attack surface mapping
├── exploitation.md                   # Initial access and attack vectors
├── privilege-escalation.md           # Local escalation and lateral movement
└── mitigation-and-recommendations.md # Defensive controls and fixes

