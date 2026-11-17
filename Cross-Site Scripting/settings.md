## Title
Reflected XSS – Mixed Security Parameter Handling

## Description

**Affected URL:** https://settings.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `academy`

The application accepts user input through several GET parameters. During testing with x8scan, multiple parameters were identified, and further assessment focused on `academy`. A crafted value containing a mixed-case script tag and a Unicode escape sequence was reflected in the response and triggered JavaScript execution in the browser.

This behavior indicates that the `academy` value is not being safely encoded before it is included in the HTML response. A JavaScript alert displaying `1` was observed during testing.

## Steps to Reproduce

1. Use x8scan to discover the GET parameters supported by https://settings.pwnedbox.site/.
2. Locate the `academy` parameter in the discovered parameter list.
3. Open the following crafted URL in a browser:

   ```text
   https://settings.pwnedbox.site/?academy=Yash%27%22%3E%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

4. Allow the page to load with the crafted value supplied through the `academy` parameter.
5. Observe the supplied input reflected in the webpage.
6. Check for a JavaScript alert displaying `1`.
7. Dismiss the alert and inspect the page source or HTML response to locate the reflected payload.

**Affected Parameter:** `academy`

**Test Payload:**

```html
Yash'"><><ScRipT>a\u006cert(1)</ScRipT>
```

## Proof of Concept (POC)

### Screenshot 1 – GET Parameter Discovery Using x8scan

<img width="959" height="323" alt="image" src="https://github.com/user-attachments/assets/c498fd85-757a-4b2f-900f-4043b8f2ef1b" />

### Screenshot 2 – Crafted Payload Reflected on the Webpage

<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/310b95a3-e07a-45ce-9332-451745f53d41" />

### Screenshot 3 – JavaScript Alert Confirming Execution

<img width="959" height="350" alt="image" src="https://github.com/user-attachments/assets/fa5a4697-662e-4c2d-bfc5-65927850811d" />

### Screenshot 4 – Reflected Payload in Page Source

<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/1e192de2-2895-4265-a816-a33fc794d7b7" />


## Impact

- **JavaScript Execution:** Run injected scripts in a visitor's browser.
- **Session Exposure:** Potentially access session-related information, depending on cookie and application protections.
- **Content Manipulation:** Modify page content or display deceptive elements.
- **Unauthorized Actions:** Potentially perform actions through a victim's session where application protections permit.

## Remediation

- **Context-Aware Output Encoding:** Encode the `academy` value for its exact HTML output context before reflecting it in the response.
- **Server-Side Validation:** Validate and safely handle all user-controlled GET parameters.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML; do not rely on filtering specific tags or JavaScript function names as the primary defense.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
