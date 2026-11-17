## Title 
Reflected XSS – String Concatenation Bypass

## Description

**Affected URL:** https://portal.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `cat`

The application accepts user-controlled input through several GET parameters. During testing with x8scan, multiple parameters were discovered. Further investigation of the `cat` parameter showed that its value is reflected in the application response without sufficient context-aware output encoding.

A crafted payload combining a mixed-case script tag with a Unicode escape sequence was used to test the input-handling behavior. The browser displayed an alert containing `1`, confirming JavaScript execution through the reflected parameter.

## Steps to Reproduce

1. Use x8scan to identify the GET parameters supported by https://portal.pwnedbox.site/.
2. Locate the `cat` parameter in the scan results.
3. Open the following crafted URL in a browser:

   ```text
   https://portal.pwnedbox.site/?cat=Yash%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

4. Allow the page to load with the crafted value supplied through the `cat` parameter.
5. Observe the supplied input reflected in the webpage.
6. Check for a JavaScript alert displaying `1`.
7. Dismiss the alert and inspect the page source or HTML response to locate the reflected payload.

**Affected Parameter:** `cat`

**Test Payload:**

```html
Yash'"><><ScRipT>a\u006cert(1)</ScRipT>
```

## Proof of Concept (POC)

### Screenshot 1 – GET Parameter Discovery Using x8scan

<img width="959" height="272" alt="image" src="https://github.com/user-attachments/assets/1f8f2e40-d5d5-4d54-9044-c6326c9b356e" />

### Screenshot 2 – Crafted Payload Reflected Through the `cat` Parameter

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/43d45e91-e6f2-4585-b384-5efd2bffa727" />

### Screenshot 3 – JavaScript Alert Confirming Execution

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/d7b46601-ead0-4ff3-a017-ed91001e05b3" />

### Screenshot 4 – Reflected Payload in Page Source

<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/e4e2a6b3-1c05-416c-8574-315f4655f9fa" />


**Observed Result:** A JavaScript alert displaying `1` appeared in the browser, confirming script execution through the `cat` parameter.

## Impact

- **JavaScript Execution:** Run injected scripts in a visitor's browser.
- **Session Exposure:** Potentially access session-related information, depending on cookie settings and application protections.
- **Content Manipulation:** Change displayed page content or introduce deceptive elements.
- **Unauthorized Actions:** Potentially perform actions through a victim's session where application protections permit.

## Remediation

- **Context-Aware Output Encoding:** Encode the `cat` value for its exact HTML output context before reflecting it in the response.
- **Server-Side Input Validation:** Validate and safely handle user-controlled GET parameter values.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML; do not rely on tag or keyword blacklists as the primary XSS defense.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
