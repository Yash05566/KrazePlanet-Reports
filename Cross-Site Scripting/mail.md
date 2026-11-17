## Title
Reflected XSS – Encoding Bypass Attempts

## Description

**Affected URL:** https://mail.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `cat`

The application reflects user-supplied data from the `cat` GET parameter in its response without sufficient context-aware output encoding. During testing, a mixed-case script tag combined with a Unicode escape sequence was used to assess the input-handling controls. The payload triggered a JavaScript alert displaying `1`, confirming script execution in the browser.

## Steps to Reproduce

1. Use x8scan to discover the GET parameters supported by https://mail.pwnedbox.site/.
2. Identify `cat` as the parameter to test.
3. Open the following crafted URL in a browser:

   ```text
   https://mail.pwnedbox.site/?cat=Yash%27%22%3E%3C%3E%3CSCripT%3Ea\u006cert(1)%3C/SCripT%3E
   ```

4. Allow the page to load with the crafted value supplied through the `cat` parameter.
5. Observe the JavaScript alert displaying `1`.
6. Dismiss the alert dialog.
7. Inspect the page source or HTML response and locate the reflected `cat` value.

**Test Payload:**

```html
Yash'"><><ScRipT>a\u006cert(1)</ScRipT>
```


## Proof of Concept (POC)

### Screenshot 1 – GET Parameter Discovery Using x8scan

<img width="959" height="283" alt="image" src="https://github.com/user-attachments/assets/a81ff318-1262-4a78-b9d9-9284044f5909" />

### Screenshot 2 – Crafted Payload Submitted Through the `cat` Parameter

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/90516a24-c9aa-4dce-a536-19bbcef1a97c" />

### Screenshot 3 – JavaScript Alert Confirming Execution

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/528874b4-32b8-4432-9120-16f2f9fcd61c" />

### Screenshot 4 – Reflected Payload in Page Source

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/5e73a04a-5ecd-4304-a198-5b64fad8dc36" />

**Affected Parameter:** `cat`


## Impact

- **JavaScript Execution:** Run injected scripts in a visitor's browser.
- **Session Risk:** Potentially expose session-related information, depending on cookie protections and application design.
- **Content Manipulation:** Modify page content or display deceptive elements.
- **Unauthorized Actions:** Potentially perform actions through a victim's session where application protections permit.

## Remediation

- **Context-Aware Output Encoding:** Encode the `cat` value for its exact HTML or JavaScript output context before reflecting it.
- **Server-Side Validation:** Validate and safely handle user-supplied GET parameter values.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML and do not rely on filtering specific tag names or JavaScript function names as the main defense.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
