# Scanning and Enumeration of DC-2 virtual machine
Initial scanning and enumeration to find exposed services and potential attack vectors

## Access Setup
In order to access the dc-2 web application I had to add  `<dc-2 ip> dc-2` to the `/etc/hosts` file, as the application uses name based hosting
## nmap
Initial scanning using nmap tool to identify possible vulnerable services on the target machine

- I first scanned all ports on TCP for version detection and OS detection using
  ```bash
  nmap -p- -sV -O <dc-2 ip>
  ```
  <img width="948" height="460" alt="image" src="https://github.com/user-attachments/assets/9cf8a9cf-bb69-44c4-bedc-13e709136bff" />

  Summary of key findings include:
    - Linux OS (3.2-4.14)
      This information was noted for potential use during post-exploitation and privilege escalation stages
    - 80/tcp - HTTP (Apache httpd 2.4.10)
      No immediate remote code execution vulnerabilities were identified for this version; however, the presence of a web service made it the primary attack surface for further enumeration
    - 7744/tcp - SSH (OpenSSH 6.7p1)
      No direct exploitation was identified. Authentication-based attacks were deprioritized until credential material could be obtained through other vectors, as brute forcing both usernames and passwords would be very noisy and impractical

## NSE Vulnerability Scanning
Additional NSE vulnerability scanning was performed
```bash
nmap --script vuln <dc-2 ip>
```
NSE scripts indicated the presence of a WordPress-based web application and disclosed several valid WordPress usernames:
- admin
- tom
- jerry
  These usernames have been added to a `wp-usernames.txt` file that could be later used for brute forcing SSH, but further Wordpress enumeration is required to obtain more information
## Wordpress Enumeration
- First ran a basic scan to check Wordpress version
    ```bash
    wpscan --url http://dc-2
    ```
    WPScan identified WordPress version 4.7.10, which contains known vulnerabilities. However, no publicly available exploits were immediately applicable in this environment
- Also enumerated Wordpress users to confirm what the NSE found
    ```bash
    wpscan --url http://dc-2 -e u
    ```
    The results were the same
## Manual Web Reconnaissance
Manual web reconnaissance was conducted to identify exposed content and identify hints relevant to exploitation
- During manual enumeration the first flag was found and the associated hint suggested the need for content based password wordlists as regular wordlists would not work here
- As the hint suggested, `cewl` was later used to build a custom wordlist
- At this stage, usernames and custom wordlists had been obtained, making SSH a viable attack vector with less noise
