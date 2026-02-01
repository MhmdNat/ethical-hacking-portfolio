# DC-3 - VulnHub - Penetration Testing Case Study
This case study documents the exploitation of the DC-3 virtual machine from VulnHub. The assessment focuses on web application enumeration, exploitation of a Joomla SQL injection vulnerability, and local privilege escalation through an outdated Linux kernel.


## Key Concepts
- Web Application Enumeration
- `SQL` Injection (CVE-2017-8917)
- Credential Exposure via Database Compromise
- Kernel-Based Privilege Escalation


## High-Level Attack Overview

### **1. Initial Enumeration**
Service enumeration identified a single exposed service running Apache on port 80. Manual inspection of the web application's source code revealed that the application was built using the `Joomla!` Content Management System. Access to the `/administrator` endpoint confirmed the CMS presence, prompting focused Joomla enumeration.


### **2. SQL Injection Exploitation**
Automated enumeration using `joomscan` identified the Joomla version as `3.7.0`, which is vulnerable to a known SQL injection flaw (`CVE-2017-8917`) in the `com_fields` component. The vulnerable parameter was exploited using `sqlmap`, allowing enumeration of backend databases. The `Joomla!` application database was identified and user credential data was extracted from the users table.


### **3. Initial Access**
Extracted password hashes were identified as `bcrypt` and successfully cracked. The recovered administrator credentials were used to authenticate to the Joomla administrator panel.  
Through template modification functionality, arbitrary PHP code execution was achieved, resulting in a reverse shell as the low‑privileged `www-data` user and establishing the initial foothold on the system.


### **4. Privilege Escalation**
Local enumeration revealed no exploitable SUID binaries, misconfigured sudo rules, or reusable credentials. Database access was limited by filesystem permissions and did not allow privilege escalation.  
The system was found to be running an outdated Linux kernel vulnerable to publicly known local privilege escalation exploits. A kernel exploit targeting the vulnerable `double-fdput()` implementation was successfully executed, escalating privileges to `root` and resulting in full system compromise.
