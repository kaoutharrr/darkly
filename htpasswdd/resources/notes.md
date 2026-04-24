# htpasswd — README.md

## Vulnerability Type

Exposed `.htpasswd` File + Weak Password Hash

---

## What is the `.htpasswd` file?

The `.htpasswd` file is an Apache authentication file used to protect directories with **Basic Authentication**.

It stores:

```txt
username:hashed_password
```

Example:

```txt
root:437394baff5aa33daa618be47b75cb49
```

Meaning:

* `root` → username
* `437394baff5aa33daa618be47b75cb49` → hashed password

Normally this file should **never be publicly accessible**, because anyone who downloads it can try to crack the hash.

---

## How the flag was found

### Step 1 — Check robots.txt

Visited:

```txt
http://192.168.56.6/robots.txt
```

It revealed:

```txt
/whatever/
```

---

### Step 2 — Browse the hidden directory

Opened:

```txt
http://192.168.56.6/whatever/
```

Inside that directory there was a file named:

```txt
htpasswd
```

Downloaded it and found:

```txt
root:437394baff5aa33daa618be47b75cb49
```

---

### Step 3 — Crack the hash

Used CrackStation to crack:

```txt
437394baff5aa33daa618be47b75cb49
```

Recovered password:

```txt
qwerty123@
```

---

### Step 4 — Access the admin panel

Then visited:

```txt
http://192.168.56.6/admin
```

Used:

```txt
Username: root
Password: qwerty123@
```

Authentication succeeded and the flag appeared.

---

## Why it works

The vulnerability happens because:

* Sensitive authentication file was exposed
* Password hash was weak enough to crack
* Same credentials were reused for admin access

An attacker can:

1. Download `.htpasswd`
2. Crack the hash
3. Reuse credentials
4. Gain access to protected pages

---

## Security issue

The server should never expose:

```txt
.htpasswd
```

Because it can lead directly to:

* credential theft
* privilege escalation
* admin access

---

## How to fix it

### Store `.htpasswd` outside web root

Move it outside:

```txt
/var/www/html/
```

---

### Block web access

Apache config:

```apache
<Files ".htpasswd">
    Require all denied
</Files>
```

---

### Use stronger password hashing

Instead of old MD5, use:

* bcrypt
* Argon2
* scrypt

---

## Summary

The issue was:

```txt
Exposed .htpasswd file → cracked password → admin access → flag
```

If you want, I can help rewrite it in a cleaner **42 Darkly writeup style** for your project notes.
