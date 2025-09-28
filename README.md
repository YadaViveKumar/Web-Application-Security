# Task 3 — Web Application Security Testing (DVWA)

This repository documents my **Web Application Security** project.  
I performed hands-on testing using **DVWA (Damn Vulnerable Web Application)** on Kali Linux to identify common web vulnerabilities and demonstrate mitigation techniques.

---

## Project Overview

The goal of this task was to practice identifying and exploiting common web application vulnerabilities, then document findings and fixes in a structured report with screenshots.  
I used **DVWA**, **Burp Suite**, and other tools to perform real-world style tests.

---

## Lab Setup

- **OS:** Kali Linux  
- **Web App:** DVWA (Damn Vulnerable Web Application)  
- **Database:** MariaDB/MySQL  
- **Tools Used:**  
  - Burp Suite Community Edition  
  - Firefox (with proxy configured)  
  - SecurityHeaders.com  
  - Apache2 + PHP  
  - Custom scripts (CSRF exploit, malicious PHP files)

---

## Vulnerabilities Tested

### 1 SQL Injection (SQLi)
- Performed SQL Injection on DVWA login and ID parameters.
- Payloads used: `1' OR '1'='1' --` etc.
- Verified database output and bypassed authentication.
- **Mitigation:** Prepared statements and parameterized queries.

### 2️ Cross-Site Scripting (XSS)
- Performed Stored XSS and Reflected XSS.
- Payloads: `<script>alert('XSS')</script>`
- Verified JavaScript execution in the browser.
- **Mitigation:** Input validation and Content Security Policy.

### 3️ Cross-Site Request Forgery (CSRF)
- Crafted a malicious HTML form to change DVWA user password without authorization.
- **Mitigation:** CSRF tokens.

### 4️ File Inclusion Attacks
- Performed Local File Inclusion (LFI) and Remote File Inclusion (RFI).
- Payload: `?page=../../../../etc/passwd` to read system files.
- **Mitigation:** Whitelisting file paths and disabling remote file inclusion.

### 5️ Burp Suite Advanced Testing
- Intercepted login requests and manipulated parameters.
- Used Intruder to fuzz inputs for vulnerabilities.
- Captured and analyzed HTTP requests/responses.

### 6️ Web Security Headers
- Analyzed headers using [SecurityHeaders.com](https://securityheaders.com).
- Configured Apache HTTP headers:
  - `X-Frame-Options`
  - `X-Content-Type-Options`
  - `X-XSS-Protection`
  - `Content-Security-Policy`
- Improved security header score.

---

## Deliverables

- **Individual Section PDFs:** Each section has its own PDF report with:
  - Explanation of the vulnerability  
  - Steps performed  
  - Commands used  
  - Screenshots of exploitation and mitigation  

- **Final Combined Report:** Consolidated all section PDFs into one comprehensive report.

- **Video Demo:** Screen-recorded demonstration of exploits and fixes.

---

## Commands Summary

Key commands used during setup and testing:

```bash
# DVWA installation
sudo apt update
sudo apt install apache2 mysql-server php php-mysqli php-gd libapache2-mod-php -y
sudo git clone https://github.com/digininja/DVWA.git /var/www/html/DVWA

# Database creation
sudo mysql -u root -p
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'dvwa';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;

# Apache headers configuration
sudo nano /etc/apache2/conf-available/security-headers.conf
sudo a2enmod headers
sudo a2enconf security-headers
sudo systemctl restart apache2
