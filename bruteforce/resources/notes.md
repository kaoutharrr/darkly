🚩 Breach: Login Brute Force / Weak Credentials
Vulnerability
Insecure Authentication — The login form is vulnerable to brute-force attacks due to the use of extremely weak, predictable credentials and a total lack of login attempt limits (rate limiting).

How to Exploit
Identify the Target: Navigate to the login page at index.php?page=signin.

Analyze the Security: Notice there is no CAPTCHA or lockout mechanism after multiple failed attempts.

Credential Guessing: Try common administrative usernames and passwords.

Execution: * Username: admin

Password: shadow

Result: Access is granted immediately, and the flag is displayed on the dashboard.

Why It Works
Weak Password Policy: The system allows the use of easily guessable passwords (like shadow).

No Rate Limiting: There is no mechanism to slow down or block a user after multiple failed login attempts, making it trivial to guess the password manually or via script.

How to Fix
Enforce Strong Passwords: Require a mix of characters and a minimum length.

Implement Account Lockout: Temporarily block an IP or account after 5 failed attempts.

Add CAPTCHA: Prevent automated scripts from spamming the login form.