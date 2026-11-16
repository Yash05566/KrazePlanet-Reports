# Reflected XSS – TutorialRepublic Search Functionality

## Description

**Target URL:** https://tutorialrepublic.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `q`

**Severity:** High

The search functionality of the website is vulnerable to Reflected XSS. User input is included in the website response without proper output encoding.

As a result, JavaScript can execute in the browser when a user opens a specially crafted URL.

## Steps to Reproduce

1. Open https://tutorialrepublic.pwnedbox.site/.
2. Go to the search functionality.
3. Enter the following payload in the search field:

   ```html
   <script>confirm(1)</script>
   ```

4. Submit the search request.
5. A JavaScript confirmation popup appears, showing that the script executed.

## Proof of Concept (POC)

### Screenshot 1 – Search Input

<!-- Add Screenshot Here -->

### Screenshot 2 – JavaScript Execution

<!-- Add Screenshot Here -->

### XSS Payload

```html
<script>confirm(1)</script>
```

### Request URL

```http
https://tutorialrepublic.pwnedbox.site/?q=%3Cscript%3Econfirm%281%29%3C%2Fscript%3E
```

## Impact

An attacker may be able to:

- Run JavaScript in a victim's browser.
- Redirect users to malicious websites.
- Display fake login forms to capture credentials.
- Perform actions through the victim's active session.
- Access information that is available to browser scripts.

**Test Result:** JavaScript execution was confirmed using the `confirm(1)` payload.

## Remediation

- Validate user input before processing it.
- Apply context-aware output encoding.
- Sanitize untrusted input where necessary.
- Avoid unsafe DOM methods such as `innerHTML`.
- Use a restrictive Content Security Policy (CSP) as an additional security layer.

## References

- https://owasp.org/www-community/attacks/xss/
- https://portswigger.net/web-security/cross-site-scripting
- https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
