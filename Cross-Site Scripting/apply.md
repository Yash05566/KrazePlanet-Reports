# Self XSS – POST-Based Reflected XSS

## Description

**Affected URL:** https://apply.pwnedbox.site/

The Name field on the application form is affected by a POST-Based Reflected Cross-Site Scripting (XSS) issue. The value submitted by the user is returned in the HTML response without being correctly output-encoded.

In testing, a crafted value was submitted through the form and the browser executed the injected JavaScript, producing an alert with the value `1`. Reviewing the response source also showed the submitted input within the page content.

## Steps to Reproduce

1. Browse to https://apply.pwnedbox.site/.
2. Find the **Name** field in the form.
3. Enter this test string:

   ```text
   Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT>
   ```

4. Submit the form by selecting **Enter**.
5. Check that a JavaScript alert appears with `1`.
6. Close the dialog by clicking **OK**.
7. Inspect the page source and find the greeting displayed by the application.
8. Verify that the submitted Name value is present in the HTML response without proper output encoding.

## POC (Proof of Concept)

**Screenshot 1 – Form input setup:** The crafted test value is placed in the Name field before the form is submitted.

<img width="1916" height="972" alt="Crafted value entered in the Name field" src="https://github.com/user-attachments/assets/6231e1d9-b185-4025-bfdb-a8c0afa56e18" />

**Screenshot 2 – Browser execution result:** The alert box appears after the form submission, showing that the injected script ran.

<img width="1918" height="962" alt="Alert shown after form submission" src="https://github.com/user-attachments/assets/29554b5c-11e6-479b-bd31-21e3240feca4" />

**Screenshot 3 – Response reflection:** The returned page source contains the submitted value in the greeting paragraph.

<img width="1916" height="966" alt="Submitted value reflected in greeting markup" src="https://github.com/user-attachments/assets/53a7a467-6cad-40d9-8e86-f0ad225a564a" />

## Impact

- It allows attackers to execute JavaScript in a user's browser.
- It may expose sensitive information or put user sessions at risk.
- It could be misused to perform unauthorized actions within the application.

## Remediation

- Apply context-aware output encoding to the Name value before including it in the HTML response.
- Validate submitted input and avoid placing untrusted data directly into executable HTML or JavaScript.
- For PHP applications, use `htmlspecialchars()` with appropriate flags and encoding when rendering text into HTML.
- Configure a restrictive Content Security Policy (CSP); use WAF rules as an additional safeguard rather than the primary XSS defense.
