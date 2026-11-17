## Title

Reflected XSS in Add Terms to the Query Box Functionality

## Description

**Affected URL:** https://pubmed.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameter:** `term`

**Description:**

A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the Add Terms to the Query Box functionality. The application accepts input through the `term` parameter and allows the submitted JavaScript to execute in the browser.

## Steps to Reproduce

1. Visit https://pubmed.pwnedbox.site/.
2. Locate the **Add terms to the query box** input field.
3. Enter the following payload:

   ```html
   <script>alert(1)</script>
   ```

4. Submit the input using the **ADD** button.
5. Observe the browser alert displaying `1`.
6. The URL shows that the submitted payload is passed through the `term` parameter.

## POC (Proof of Concept)

### Screenshot 1 – Payload Entered in Add Terms Field

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/fb496fb0-5bc5-4730-be12-3fbd155a9b2f" />


### Screenshot 2 – JavaScript Alert Popup

<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/94036b79-a36d-45f4-b396-9091ac43f1c1" />


## Impact

- Execute JavaScript in a user's browser.
- Display misleading content or fake login forms..
- Perform actions through the victim's active session, if the application allows it.

## Remediation

- Encode user-controlled output before displaying it in the page.
- Validate and sanitize input where appropriate.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.
