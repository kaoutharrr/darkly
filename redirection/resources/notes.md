# redirection — README.md

## Vulnerability Type

Open Redirect / Parameter Manipulation

---

## Description

The application contains a redirect feature that uses a URL parameter to decide where the user should be sent.

The page uses:

```txt id="yl6om5"
site=facebook
```

inside the URL.

Because the application trusts this parameter without proper validation, changing its value can trigger unintended behavior and reveal the flag.

---

## How the vulnerability was discovered

### Step 1 — Click the social media icon

Clicked the Facebook icon on the page.

It redirected to:

```txt id="ddq6x9"
http://192.168.56.6/index.php?page=redirect&site=facebook
```

---

### Step 2 — Analyze the URL

The interesting part was:

```txt id="s7lyqo"
site=facebook
```

This indicates the application uses the `site` parameter to determine the redirect target.

---

## Exploitation

### Step 3 — Modify the parameter

Changed:

```txt id="f0m2nw"
site=facebook
```

to another value manually in the browser URL.

Example:

```txt id="bjpfvi"
http://192.168.56.6/index.php?page=redirect&site=test
```

After loading the modified URL, the application returned the flag.

---

## Why it works

The server likely performs logic similar to:

```php id="b6m4ib"
$site = $_GET['site'];
redirect_user($site);
```

The problem is that the server trusts user input directly from:

```txt id="y2n63r"
$_GET['site']
```

without strict validation.

---

## What is an Open Redirect?

An open redirect happens when a website lets the user control a redirect destination.

Example vulnerable code:

```php id="l8mjly"
header("Location: " . $_GET['url']);
```

An attacker can change:

```txt id="f5sqq4"
url=
```

to any destination.

---

## Security issue

The application trusted:

```txt id="l7ow84"
A user-controlled URL parameter
```

for navigation logic.

This can lead to:

* phishing attacks
* malicious redirects
* access control bypass
* information disclosure

---

## Impact

An attacker may:

* redirect users to malicious websites
* manipulate internal application flow
* bypass protections
* reveal sensitive data

---

## How to fix

The application should:

* use a whitelist of allowed destinations
* reject unknown values
* never redirect directly from user input

Example secure validation:

```php id="gaj6np"
$allowed = ["facebook", "twitter", "instagram"];
```

Only approved values should be accepted.

---

## Summary

```txt id="w9h34o"
Facebook link clicked
→ redirect URL discovered
→ site parameter modified
→ server trusted input
→ flag revealed
```
