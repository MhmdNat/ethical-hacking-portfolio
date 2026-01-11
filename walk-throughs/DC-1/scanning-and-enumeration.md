# Scanning and Enumeration of DC-1 virtual machine
This section will contain basic scanning and enumeration of services running on the target virtual machine

## nmap
This section will contain scans with nmap

- I first ran a TCP scan with version detection on all ports of the target machine
  <img width="826" height="437" alt="image" src="https://github.com/user-attachments/assets/6696e1ec-72a4-4aa4-ae1e-c97c85fae071" />


  key findings include:
  - OS: Linux (3.2 - 3.16)
  - 22: SSH (OpenSSH 6.0p1 Debian)
  - 80: HTTP (Apache httpd 2.2.22)
    
- I then ran nmap's general vulnerability script
  <img width="766" height="73" alt="image" src="https://github.com/user-attachments/assets/3aa917b8-3572-4dcf-beec-47d310ff35c5" />
  <img width="1058" height="411" alt="image" src="https://github.com/user-attachments/assets/8510774d-88a2-4496-a074-7db180b031d2" />
  The machine is vulnerable to a CVE

