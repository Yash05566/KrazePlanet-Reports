## Title

Reflected XSS - GIGW: Government Website Guidelines Portal

## Description

**Affected URL:** https://guidelines.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameter:** `s`

**Description:**

A Reflected Cross-Site Scripting (XSS) vulnerability was found in the search functionality of the website. The application accepts input through the `s` parameter and allows the supplied JavaScript to execute in the browser.

## Steps to Reproduce

1. Visit https://guidelines.pwnedbox.site/.
2. Locate the search box at the top of the page.
3. Enter the following payload:

   ```html
   <script>alert(1)</script>
   ```

4. Submit the search request.
5. Observe the browser alert displaying `1`.
6. Check the URL and notice that the payload is passed through the `s` parameter.

## Proof of Concept (POC)

### Screenshot 1 – Payload Entered in Search Box

<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/9af6ea7f-dfca-4e1d-8b06-6763272b46fd" />


### Screenshot 2 – JavaScript Alert Popup

<img width="940" height="496" alt="image" src="https://github.com/user-attachments/assets/aa8e7679-0bc4-4ef2-83fb-33356c3d3fe5" />


### HTTP Request

```http
https://guidelines.pwnedbox.site/index.php?s=%3Cscript%3Ealert%281%29%3C%2Fscript%3E
```

### Payload

```html
<script>alert(1)</script>
```

## Impact

Depending on the application's functionality, this vulnerability could allow an attacker to:

- Execute JavaScript in a user's browser.
- Display fake content or login forms.
- Redirect users to malicious websites.
- Perform actions through the victim's active session, if the application allows it.

**Actual Impact:** The `alert(1)` payload successfully executed and displayed a browser popup containing `1`. No further impact was tested.

**Affected Users:** Users who open a specially crafted URL containing the malicious search parameter.


## Remediation

- Encode user-controlled output before displaying it in the page.
- Validate and sanitize input where appropriate.
- Avoid unsafe DOM manipulation methods such as `innerHTML`.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

