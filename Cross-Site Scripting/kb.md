# Reflected XSS – Search Filter Bypass

## Description

**Affected URL:** https://kb.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `search`

The application reflects user-controlled input from the `search` GET parameter in the results heading and empty-state message. During testing, x8scan was used to identify the available GET parameters, including parameters that may not be immediately visible through the page interface. Further testing of the `search` parameter showed that its value was reflected without suitable context-aware output encoding.

A crafted payload containing a mixed-case script tag and a Unicode escape sequence triggered JavaScript execution in the browser, confirmed by an alert displaying `1`.

## Steps to Reproduce

1. Run x8scan against https://kb.pwnedbox.site/ to discover the application's available GET parameters.
2. Review the scan output and identify the `search` parameter for further testing.
3. Use the following crafted URL to submit the test payload through `search`:

   ```text
   https://kb.pwnedbox.site/?search=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

4. Open the URL in a browser and allow the page to load.
5. Observe the JavaScript alert displaying `1`.
6. Dismiss the alert dialog.
7. Inspect the page source or HTML response and locate the reflected value in the results heading and empty-state message.

## Proof of Concept (POC)

### Screenshot 1 – x8scan Output Showing Discovered GET Parameters

<!-- Add Screenshot 1 here: x8scan scan output showing the discovered GET parameters, including search. -->

### Screenshot 2 – Crafted Payload and JavaScript Alert

<!-- Add Screenshot 2 here: crafted search URL and alert dialog displaying 1. -->

### Screenshot 3 – Reflected Payload in Page Source

<!-- Add Screenshot 3 here: page source showing the search value reflected in the results heading and empty-state message. -->

**Affected Parameter:** `search`

**Test Payload:**

```html
Priyanshxop'"><><ScRipT>a\u006cert(1)</ScRipT>
```

**Observed Result:** The browser displayed an alert containing `1`, confirming JavaScript execution through the reflected `search` parameter.

## Impact

- **Session Hijacking Risk:** Injected scripts may expose or misuse session-related data, depending on cookie protections.
- **Account Compromise:** Successful exploitation could increase the risk of unauthorized access to user accounts.
- **Unauthorized Actions:** Scripts may perform actions within the application using a victim's active session, subject to application controls.

The demonstrated test confirms JavaScript execution. Session hijacking, account takeover, and unauthorized actions were not independently verified.

## Remediation

- **Output Encoding:** Contextually encode search input before displaying it in the results heading or empty-state message.
- **Input Validation:** Validate and safely process values received through the `search` GET parameter.
- **Safe HTML Handling:** Do not rely on blocking individual HTML tags or JavaScript function names; avoid rendering untrusted input as executable markup.
- **Content Security Policy:** Deploy a restrictive CSP as an additional safeguard against script execution.
