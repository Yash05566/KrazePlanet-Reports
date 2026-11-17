## Title

Reflected XSS - RefSeek: Academic & Scientific Search Engine

## Description

**Affected URL:** https://refseek.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameter:** `q`

**Description:**

A Reflected Cross-Site Scripting (XSS) vulnerability was found in the search functionality of the website. The application accepts input through the `q` parameter and allows the submitted JavaScript to execute in the browser.

## Steps to Reproduce

1. Visit https://refseek.pwnedbox.site/.
2. Locate the search box on the homepage.
3. Enter the following payload:

   ```html
   <script>alert(1)</script>
   ```

4. Submit the search request.
5. Observe the browser alert displaying `1`.
6. Check the URL and notice that the payload is passed through the `q` parameter.

## POC (Proof of Concept)

### Screenshot 1 – Payload Entered in Search Box

<img width="940" height="495" alt="image" src="https://github.com/user-attachments/assets/289b828c-3c8c-4841-9a61-f9fd34bf3c05" />


### Screenshot 2 – JavaScript Alert Popup

<img width="940" height="495" alt="image" src="https://github.com/user-attachments/assets/fc06df4f-cd93-4a94-bd45-324267aee598" />


## Impact

- Execute JavaScript in a user's browser.
- Display misleading content or fake login forms..
- Perform actions through the victim's active session, if the application allows it.

## Remediation

- Encode user-controlled output before displaying it in the page.
- Validate and sanitize input where appropriate.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.
