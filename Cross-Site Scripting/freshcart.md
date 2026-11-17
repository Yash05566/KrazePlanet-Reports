## Title

Reflected Cross-Site Scripting in FreshCart Search

## Description

**Affected URL:** https://freshcart.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `q`

The search feature does not safely handle the value supplied by the user. During testing, a script entered into the search field was processed through the `q` parameter and ran in the browser. This indicates that the input is being reflected without suitable output encoding.

## Steps to Reproduce

1. Open the FreshCart website: https://freshcart.pwnedbox.site/.
2. Click the search field on the homepage.
3. Enter this test payload:

   ```html
   <script>alert(1)</script>
   ```

4. Submit the search.
5. Check the browser window. An alert box displaying `1` appears.
6. Confirm that the address bar contains the submitted value in the `q` parameter.

## POC (Proof of Concept)

### Screenshot 1 – Search Field Containing the Payload

<img width="940" height="496" alt="image" src="https://github.com/user-attachments/assets/a6dc65c5-4079-4b51-9711-60b45016dd8a" />


### Screenshot 2 – Alert Generated After Submission

<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/287882e3-8989-4b6a-9101-edb3916ae303" />

## Impact

- Execute JavaScript in a user's browser.
- Display misleading content or fake login forms..
- Perform actions through the victim's active session, if the application allows it.

## Remediation

- Encode user-controlled output before displaying it in the page.
- Validate and sanitize input where appropriate.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

