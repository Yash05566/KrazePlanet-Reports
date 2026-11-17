## Title
Reflected-XSS 

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

### Request URL

```text
https://tutorialrepublic.pwnedbox.site/?q=<script>alert(1)</script>
```

## Impact

- Execute arbitrary JavaScript within the context of the vulnerable website.
- Modify the webpage's content or display misleading information.
- Account Takeover.

**Observed Result:** The `alert(1)` popup appeared after submitting the payload. This confirms JavaScript execution. Further impact was not tested.

## Remediation

- Apply context-aware HTML encoding to all user-controlled input before displaying it in the response.
- Validate and sanitize input where required.
- Avoid inserting untrusted input into HTML using unsafe methods such as `innerHTML`.
- Apply a suitable Content Security Policy (CSP) as an additional security measure.
