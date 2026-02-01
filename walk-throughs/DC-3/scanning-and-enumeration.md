# Scanning and Enumeration of DC-3 VM
This phase focuses on the enumeration of the web application using manual and automated tools to identify application-level vulnerabilities.


## Initial Scanning of Exposed Services
An initial service-version enumeration identifying exposed services was performed. All ports were scanned to verify whether a service was open on an unconventional port, effectively hiding it from normal scans which scan the top 1000 used ports.
```bash
nmap -sV -p- <dc-3 ip>
```
Open ports are:
- 80/tcp - `HTTP` (Apache httpd 2.4.18)

As no other ports were open, further enumeration to the web application was performed.

## Manual Web Application Enumeration
After reading the generator meta tag in the source code of the main page, the web application was identified as built using the `Joomla!` Content Management System. Visiting the `Joomla!` admin page on `/administrator` confirmed the presence of the `Joomla!` CMS.


Thus, following these findings, automated enumeration was performed using `joomscan`.


## Automated Web Application Enumeration
The automated `Joomla!` scanning tool was used on the target machine:
```bash
joomscan http://<dc-3 ip>
```
The most critical finding was identifying the `Joomla!` version as `3.7.0`, a very outdated version of the CMS, which was the pivot point for CVE research.


## CVE Research
Searching for the `Joomla! 3.7.0` on [ExploitDb](https://www.exploit-db.com/exploits/42033) resulted in finding an exploit for `CVE 2017-8917`. 


This CVE targets the `com_fields` component in core `Joomla! 3.7.0` at the specific URL: `http://<dc-3 ip>/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml'`. It allows arbitrary SQL query manipulation via error-based `SQL` injection.


Before exploitation takes place, manual confirmation was performed to ensure the presence of the vulnerability on the target system. Visiting the URL at on the machine resulted in an `SQL` error, confirming that user-input is executed in server-side queries without proper sanitization, directly translating into an injectable parameter. Hence, exploitation was the next step.
