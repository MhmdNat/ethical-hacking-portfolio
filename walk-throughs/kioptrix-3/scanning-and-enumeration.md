# Scanning and Enumeration of Kioptrix-3 VM
This phase focuses on identifying exposed services and enumerating the web application to find possible attack vectors.

## Initial Scanning of Exposed Services
An initial service-version scan was conducted using `nmap`.
```bash
nmap -Pn -sV <kioptrix-3 ip>
```
Open ports are:
  - 22/tcp - `SSH` (OpenSSH 4.7p1)
  - 80/tcp - `HTTP` (Apache httpd 2.2.8, PHP/5.2.4)

Further enumeration focused on the `HTTP` service to identify application level vulnerabilities.

## Web Application Enumeration
Three main pages were found: `home`, `blog`, and `login`

### Home Page
- A `page` parameter was exposed in the URL which allowed users to manually change the requested page. However, this did not lead to file inclusion.

### Blog Page
- The ability to comment on blogs was noted for possible `PHP` server-code injection but was ruled out after further testing.

  
- The application's gallery was reached through the provided link `http://kioptrix3.com/gallery`.

### Login Page
- Preliminary `SQLi` was tested on this page but no injection was confirmed.

### Gallery Page
- Allowed viewing of galleries and uploaded photos.

  
- While reading source code, specifically in the links section, a commented link was found `http://kioptrix3.com/gallery/index.php/gadmin` which allowed viewing another login form with no injection confirmed.

  
- Using the network tab in the developer tools, a javascript file was found `javascript.js`. This file contained functions that communicate with server endpoints which in turn access the database.

### `deleteGallery` Function
- In this javascript function, the following path was found `gallery.php?task=delete&id=`. The full path was discovered as `http://kioptrix3.com/gallery/gallery.php?task=delete&id=`. While testing this path it was found that as an unauthenticated user, the server did not allow deletion of a gallery, but instead returned the gallery containing uploaded photos.


- To evaluate whether the `id` parameter was properly handled by the backend, manual input validation testing was performed. Supplying a single quote character (`'`) as the value of the `id` parameter resulted in a database error being returned by the server.


- This behavior indicates that user-supplied input is directly incorporated into an SQL query without proper sanitization or parameterization, confirming the presence of an `SQL` injection vulnerability in the `id` parameter. Further exploitation of this issue was deferred to the exploitation phase.
