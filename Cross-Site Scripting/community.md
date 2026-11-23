## Title

Stored Cross-Site Scripting (XSS) in Forum Profiles and Discussion Comments

## Description

**Affected URL:** https://community.pwnedbox.site/

Stored Cross-Site Scripting (XSS) was identified in the Community Hub application. User-supplied content entered through registration/profile fields and discussion comments is saved and later displayed without adequate output encoding. When the affected profile or discussion content is viewed, the injected HTML can trigger JavaScript in the viewer's browser.

## Steps to Reproduce

1. Visit https://community.pwnedbox.site/ and open the registration page.
2. Provide a valid email address and complete the required registration details.
3. Place the following payload in a profile field controlled by the user, such as First Name, About Me, or Signature:

   ```html
   Priyansh'"><><img src=x onerror=confirm(1)>
   ```

4. Submit the form and confirm that the account is created.
5. Open the newly created profile and observe the JavaScript confirmation dialog caused by the saved input.
6. Sign in and open a discussion post.
7. Enter the following payload in the comment field:

   ```html
   Priyansh'"><><img src=x onerror=confirm('Policies')>
   ```

8. Submit the comment.
9. Reload or revisit the discussion page. The confirmation dialog appears when the saved comment is displayed.

## POC (Proof of Concept)

**Screenshot 1 – Injected input in registration fields**

<!-- Add Screenshot 1 here: Show the HTML injection payloads entered into multiple user-controlled registration/profile fields. -->

**Screenshot 2 – Account creation completed**

<!-- Add Screenshot 2 here: Show the successful registration response after submitting the profile data. -->

**Screenshot 3 – Script execution on the profile page**

<!-- Add Screenshot 3 here: Show the JavaScript confirmation dialog triggered by the stored profile input. -->

**Screenshot 4 – Unencoded HTML in the profile source**

<!-- Add Screenshot 4 here: Show the profile page source or rendered markup containing the injected HTML without proper output encoding. -->

**Screenshot 5 – XSS payload submitted as a discussion comment**

<!-- Add Screenshot 5 here: Show the payload entered into the discussion comment form before submission. -->

**Screenshot 6 – Script execution from the saved comment**

<!-- Add Screenshot 6 here: Show the JavaScript confirmation dialog displayed when the stored discussion comment is viewed. -->

**Screenshot 7 – Stored payload in discussion data**

<!-- Add Screenshot 7 here: Show the discussion data or page source containing the saved comment payload. -->

## Impact

- An attacker may execute JavaScript in the browser of a user who views the affected profile or discussion content.
- The script may expose information available to the vulnerable page or interfere with a user's interaction with the application.
- Depending on the application's functionality and the affected user's privileges, the issue may enable unauthorized actions through the user's session.

## Remediation

- Apply context-appropriate output encoding whenever profile fields and discussion comments are rendered.
- If the application intentionally permits HTML, sanitize submitted content with a maintained allowlist-based HTML sanitizer.
- Avoid inserting untrusted content directly into HTML, attributes, or executable JavaScript contexts.
- Deploy a restrictive Content Security Policy (CSP) as an additional security layer; do not rely on a WAF as the primary XSS defense.
