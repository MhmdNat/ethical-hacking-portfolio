# Scanning and Enumeration of DC-1 virtual machine
Initial scanning and enumeration to find exposed services and potential attack vectors

## nmap

- I first ran a TCP scan with version detection on all ports of the target machine
  ```bash
  nmap -p- -sV -O <dc1-ip>
  ```
  <img width="826" height="437" alt="image" src="https://github.com/user-attachments/assets/6696e1ec-72a4-4aa4-ae1e-c97c85fae071" />


  The following services were identified as potential attack vectors
  - 22/tcp - SSH (OpenSSH 6.0p1 Debian)
    Relatively old, but no direct attack vectors were identified at this stage, brute force could be used; however that is noisy and impractical at this stage
  - 80/tcp - HTTP (Apache httpd 2.2.22)
    Known to host vulnerable web applications, needs further enumeration
    
- I then ran nmap's general vulnerability script
    ```bash
   nmap --script vuln <dc1-ip>
    ```
  <img width="1058" height="411" alt="image" src="https://github.com/user-attachments/assets/8510774d-88a2-4496-a074-7db180b031d2" />
  The scan suggests that the machine is vulnerable to "CVE-2014-3704", an SQL injection vulnerability affecting Drupal, which can be leveraged to achieve remote code execution in certain configurations. This is a more promising initial access vector than SSH, and will be attempted using the MetaSploit Framework later in the walkthrough

## Web Enumeration
After visiting the web application and reviewing the source code, specifically the meta tag, I found that it is hosted using Drupal 7 indicating that it is vulnerable to said CVE which affects versions 7-7.31 (included) of Drupal.

Since the version was confirmed, and the vulnerability allowed unauthenticated exploitation, addition directory busting was deemed unnecessary and focus shifted towards exploitation

If the msf exploit fails further enumeration would include, manual discovery of robots.txt and directory brute forcing to map drupal endpoints

