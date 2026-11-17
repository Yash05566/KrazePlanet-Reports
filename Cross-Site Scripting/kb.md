# Reflected XSS – Search Filter Bypass

## Description

**Affected URL:** https://kb.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `search`

The search feature reflects user-provided input in the results heading and empty-state message. During testing, the reflected value was not properly encoded for its HTML context. A crafted payload containing a mixed-case script tag and a Unicode escape sequence triggered JavaScript execution in the browser, confirming that the `search` parameter can introduce executable content into the response.

## Steps to Reproduce

1. Open the following URL in a browser:

   ```text
   https://kb.pwnedbox.site/?search=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

2. Alternatively, enter an ordinary search term in the search field and submit it.
3. Check the resulting URL and identify the `search` GET parameter.
4. Replace its value with the crafted payload:

   ```text
   Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

5. Load the modified URL in the browser.
6. Observe the JavaScript alert displaying `1`.
7. Dismiss the alert and inspect the page source or HTML response.
8. Check how the supplied value appears in the results heading and empty-state message.

## Proof of Concept (POC)

### Screenshot 1 – Search Field with a Test Value

<!-- Add Screenshot 1 here: search field populated with a normal test value. -->

### Screenshot 2 – Crafted Search URL and JavaScript Alert

<!-- Add Screenshot 2 here: crafted search parameter and alert displaying 1. -->

### Screenshot 3 – Reflected Payload in the Page Source

<!-- Add Screenshot 3 here: source showing the reflected value in the results heading and empty-state message. -->

**Affected Parameter:** `search`

**Test Payload:**

```html
Priyanshxop'"><><ScRipT>a\u006cert(1)</ScRipT>
```

**Observed Result:** The browser displayed an alert containing `1`, confirming JavaScript execution through the reflected search input.

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
