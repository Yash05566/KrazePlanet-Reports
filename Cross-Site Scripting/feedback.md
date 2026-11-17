## Title

Reflected XSS in GET Parameter Reflection Form

## Description

**Affected URL:** https://feedback.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameters:** `fname`, `lname`

The page accepts First Name and Last Name values through GET parameters. Testing showed that script code placed in these fields is executed when the submitted request is loaded. This indicates that the reflected values are not being safely encoded before they reach the browser.

## Steps to Reproduce

1. Open https://feedback.pwnedbox.site/.
2. Enter the following value in the **First Name** field:

   ```html
   "><sCripT>alert(1)</sCripT>
   ```

3. Enter the following value in the **Last Name** field:

   ```html
   "><sCripT>alert(2)</sCripT>
   ```

4. Submit the form.
5. Observe the JavaScript alert popups. The test displays `1` and `2` in the browser.

## POC (Proof of Concept)

### Screenshot 1 – Form Fields with Test Payloads

<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/918d97bc-4584-42a8-9e41-8a8ec59bc2d5" />


### Screenshot 2 – JavaScript Alert

<img width="940" height="213" alt="image" src="https://github.com/user-attachments/assets/501626f9-ff96-4bd2-b84c-f8afbe20d5a7" />


### Screenshot 3 – Second JavaScript Alert

<img width="940" height="361" alt="image" src="https://github.com/user-attachments/assets/62dbcbcc-4bbd-4dcd-ad1d-3dce315c5e4a" />


## Impact

- Run injected JavaScript in a user's browser.
- Alter webpage elements and form fields.
- Use malicious content for phishing attempts.
- Trigger the vulnerability through manipulated URLs.

## Remediation

- Encode `fname` and `lname` for the correct HTML output context before displaying them.
- Perform proper server-side input validation.
- Use allowlist validation instead of blacklist-based filtering.
- Configure a strict Content Security Policy (CSP).
