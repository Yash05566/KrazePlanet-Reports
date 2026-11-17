# Reflected XSS – Encoding Bypass Attempts

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
   https://mail.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

4. Allow the page to load with the crafted value supplied through the `cat` parameter.
5. Observe the JavaScript alert displaying `1`.
6. Dismiss the alert dialog.
7. Inspect the page source or HTML response and locate the reflected `cat` value.

## Proof of Concept (POC)

### Screenshot 1 – GET Parameter Discovery Using x8scan

<!-- Add Screenshot 1 here: x8scan output showing the available GET parameters. -->

### Screenshot 2 – Crafted Payload Submitted Through the `cat` Parameter

<!-- Add Screenshot 2 here: browser showing the crafted URL or submitted payload. -->

### Screenshot 3 – JavaScript Alert Confirming Execution

<!-- Add Screenshot 3 here: alert dialog displaying 1. -->

### Screenshot 4 – Reflected Payload in Page Source

<!-- Add Screenshot 4 here: page source showing the reflected input. -->

**Affected Parameter:** `cat`

**Test Payload:**

```html
Priyanshxop'"><><ScRipT>a\u006cert(1)</ScRipT>
```

**Observed Result:** The browser displayed a JavaScript alert containing `1`, confirming that the supplied input resulted in JavaScript execution.

## Impact

- **JavaScript Execution:** Run injected scripts in a visitor's browser.
- **Session Risk:** Potentially expose session-related information, depending on cookie protections and application design.
- **Content Manipulation:** Modify page content or display deceptive elements.
- **Unauthorized Actions:** Potentially perform actions through a victim's session where application protections permit.

The alert confirms script execution; session hijacking, account takeover, and unauthorized actions were not independently tested.

## Remediation

- **Context-Aware Output Encoding:** Encode the `cat` value for its exact HTML or JavaScript output context before reflecting it.
- **Server-Side Validation:** Validate and safely handle user-supplied GET parameter values.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML and do not rely on filtering specific tag names or JavaScript function names as the main defense.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
