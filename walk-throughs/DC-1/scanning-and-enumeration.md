# Scannin and Enumeration of Kioptrix level 1
This section will contain basic scanning and enumeration of services running on the target virtual machine

## nmap
This section will contain scans with nmap

I first ran a TCP scan with version detection on all ports of the target machine
<img width="1223" height="421" alt="image" src="https://github.com/user-attachments/assets/2cad3237-9482-422c-b01e-e96d17ea768f" />
key services include:
- 22: SSH
- 80: HTTP
- 139: netbios-ssn (samba)
- 443: HTTPS

continue with smb for now try to enumerate tomorrow..
