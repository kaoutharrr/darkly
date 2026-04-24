# Darkly

## Project Overview

**Darkly** is a web security project at **42** that introduces common web vulnerabilities through a vulnerable website.
The goal is to identify weaknesses, understand how they work, and exploit them in a safe environment to improve security knowledge.

This project focuses on:

* Web application security basics
* Vulnerability discovery
* Exploitation techniques
* Secure development awareness

---

## My Contribution

During this project, I worked on the following vulnerabilities:

### Bruteforce

Tested weak authentication by automating login attempts to discover valid credentials and understand password security weaknesses.

### htpasswdd

Analyzed exposed password files and recovered credentials from improperly protected `.htpasswd` data.

### Recover

Explored insecure password recovery mechanisms that allowed user account information leakage.

### SQL Injection (members)

Injected SQL payloads into the members page to extract hidden database information.

### Hidden

Investigated hidden files and directories to uncover sensitive information left accessible on the server.

### README.md

Reviewed exposed documentation files that revealed internal project structure and useful hints.

### Redirection

Identified an open redirect vulnerability that allowed external URL redirection without validation.

### XSS Media

Exploited a Cross-Site Scripting vulnerability in the media section by injecting malicious client-side code.

---

## What I Learned

Through this project, I learned:

* How common web vulnerabilities work in practice
* How attackers enumerate weak points in web applications
* The importance of input validation
* Why sensitive files should never be publicly exposed
* How authentication systems can fail when poorly designed
* The risks of trusting user input without sanitization
* Better understanding of offensive security methodology

---

## Tools Used

* Browser developer tools
* Burp Suite
* curl / wget
* SQL payloads
* Basic scripting for testing

---

## Conclusion

Darkly helped me better understand real-world web vulnerabilities by applying both theory and practice.
It strengthened my ability to analyze web applications and recognize insecure implementations before they become serious security issues.
