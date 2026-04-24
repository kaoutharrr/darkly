# recover — README.md

## Vulnerability Type

Parameter Tampering / Broken Password Recovery

---

## Description

The password recovery page trusts a hidden form field containing the email address used for recovery.

Because the hidden field can be modified in the browser, an attacker can change its value and bypass the intended recovery process.

---

## How the vulnerability was discovered

### Step 1 — Open the password recovery page

Visited the password reset page from:

```txt id="prw5t1"
Forgot Password
```

---

### Step 2 — Inspect the form

Opened browser developer tools and inspected the HTML form.

The page contained a hidden input similar to:

```html id="t7t4cu"
<input type="hidden" name="mail" value="webmaster@borntosec.com">
```

This means the server uses the value of:

```txt id="cq6bcm"
mail
```

during the recovery request.

---

## Why this is vulnerable

A hidden field is invisible in the page, but it can still be modified by the user.

Example:

```html id="ljp5cm"
<input type="hidden">
```

Hidden does **not** mean secure.

---

## Exploitation

### Step 3 — Modify the hidden field

Changed the email value inside the browser from:

```txt id="wmx7lq"
webmaster@borntosec.com
```

to another value before submitting the form.

Example:

```html id="26mzn7"
<input type="hidden" name="mail" value="attacker@fake.com">
```

---

### Step 4 — Submit the form

Clicked:

```txt id="eh0zba"
Submit
```

The application accepted the modified value and immediately revealed the flag.

---

## Why it works

The server likely trusted the client input:

```php id="pjcl1n"
$email = $_POST['mail'];
```

Instead of verifying the correct email on the server side.

Because the client controls the form, the value can be changed.

---

## Security issue

The application relied on:

```txt id="m1t1o2"
User-controlled hidden input
```

for a sensitive action.

That creates a **parameter tampering** vulnerability.

---

## Impact

An attacker could:

* manipulate password recovery
* bypass intended validation
* trigger unauthorized actions
* access protected information

---

## How to fix

The server should:

* never trust hidden fields
* validate recovery data server-side
* store email internally in session
* verify ownership before processing reset requests

Example secure approach:

```txt id="v9c0ij"
User requests reset
→ server generates token
→ token emailed
→ server validates token
```

---

## Summary

```txt id="kqszx4"
Hidden mail field discovered
→ field modified
→ form submitted
→ server trusted modified input
→ flag revealed
```
