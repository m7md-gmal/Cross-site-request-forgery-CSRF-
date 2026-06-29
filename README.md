## What is CSRF (Cross-Site Request Forgery)?

**Cross-Site Request Forgery (CSRF)** is a web security vulnerability that allows an attacker to induce authenticated users to perform unintended actions on a target website.

### How it works:
1. The victim is logged into the target application (valid session cookies).
2. The attacker tricks the victim into visiting a malicious page (via link, email, etc.).
3. The malicious page automatically sends a forged request to the target site.
4. The target site executes the request because it trusts the victim's cookies.

### Common Exploitable Actions:
- Changing email address
- Changing password
- Making money transfers
- Deleting account

### Exploitation Techniques:
- Auto-submitting HTML forms
- Using JavaScript (`fetch`, `XMLHttpRequest`)
- Bypassing defenses (CSRF tokens, SameSite, Referer, Origin)
- Clickjacking (in some cases)
