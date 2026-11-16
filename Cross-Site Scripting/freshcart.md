## Title

Reflected XSS in User Search Functionality

## Description

**Affected URL:** https://freshcart.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameter:** `q`

**Severity:** [To be assessed]

**Description:**

A Reflected Cross-Site Scripting (XSS) vulnerability was found in the search functionality of the website. The application accepts input through the `q` parameter and allows the submitted JavaScript to execute in the browser.

## Steps to Reproduce

1. Visit https://freshcart.pwnedbox.site/.
2. Locate the search box on the homepage.
3. Enter the following payload:

   ```html
   <script>alert(1)</script>
   ```

4. Submit the search request.
5. Observe the browser alert displaying `1`.
6. The URL shows that the payload is passed through the `q` parameter.

## POC (Proof of Concept)

### Screenshot 1 – JavaScript Alert Popup

<!-- Upload Screenshot 1 Here -->

### Screenshot 2 – Search Field with Payload

<!-- Upload Screenshot 2 Here -->

### HTTP Request / URL

```http
https://freshcart.pwnedbox.site/index.php?q=%3Cscript%3Ealert%281%29%3C%2Fscript%3E
```

### Payload

```html
<script>alert(1)</script>
```

## Impact

Depending on the application's functionality, this vulnerability could allow an attacker to:

- Execute JavaScript in a user's browser.
- Display misleading content or fake login forms.
- Redirect users to malicious websites.
- Perform actions through the victim's active session, if the application allows it.

**Actual Impact:** The `alert(1)` payload successfully executed and displayed a browser popup containing `1`. No further impact was tested.

**Affected Users:** Users who open a specially crafted URL containing the malicious search parameter.

## Remediation

- Encode user-controlled output before displaying it in the page.
- Validate and sanitize input where appropriate.
- Avoid unsafe DOM manipulation methods such as `innerHTML`.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.
