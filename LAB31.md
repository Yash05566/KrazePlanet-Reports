## Title
Reflected XSS 

## Description

**Affected URL:** https://tutorialrepublic.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `q`

The search parameter `q` accepts user input that is reflected in the page response without proper handling. By submitting an HTML script payload, JavaScript executes in the browser.

## Steps to Reproduce

1. Open https://tutorialrepublic.pwnedbox.site/.
2. Enter the following payload in the search box:

   ```html
   <script>alert(1)</script>
   ```

3. Submit the search.
4. The browser displays an alert popup with the value `1`.
5. This confirms that the supplied JavaScript was executed.

## Proof of Concept (POC)

### Screenshot 1 – Payload Entered in Search Box

<img width="940" height="494" alt="image" src="https://github.com/user-attachments/assets/fcba3e91-72f4-43ab-8571-12265b3f8bd2" />


### Screenshot 2 – Alert Popup After Submission

<img width="940" height="497" alt="image" src="https://github.com/user-attachments/assets/3103fe66-21b0-49e3-8028-736970a7d093" />


### Payload

```html
<script>alert(1)</script>
```

### Request URL

```text
https://tutorialrepublic.pwnedbox.site/?q=<script>alert(1)</script>
```

## Impact

An attacker could create a specially crafted URL and trick a user into opening it. If the user visits the URL, the injected JavaScript may run in their browser.

Depending on the affected page and the victim's session, this could be used to:

- Display fake content or login forms.
- Redirect the user to another website.
- Perform actions as the victim where the application allows it.
- Access information available to JavaScript in the page.

**Observed Result:** The `alert(1)` popup appeared after submitting the payload. This confirms JavaScript execution. Further impact was not tested.

## Remediation

- Encode user-controlled data before displaying it in the page.
- Validate and sanitize input where required.
- Avoid inserting untrusted input into HTML using unsafe methods such as `innerHTML`.
- Apply a suitable Content Security Policy (CSP) as an additional security measure.
