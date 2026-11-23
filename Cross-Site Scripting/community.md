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
   Yash'"><><img src=x onerror=confirm(1)>
   ```

4. Submit the form and confirm that the account is created.
5. Open the newly created profile and observe the JavaScript confirmation dialog caused by the saved input.
6. Sign in and open a discussion post.
7. Enter the following payload in the comment field:

   ```html
   Yash'"><><img src=x onerror=confirm('Policies')>
   ```

8. Submit the comment.
9. Reload or revisit the discussion page. The confirmation dialog appears when the saved comment is displayed.

## POC (Proof of Concept)

**Screenshot 1 – Injected input in registration fields**

<img width="959" height="508" alt="image" src="https://github.com/user-attachments/assets/53a3f026-a149-48f7-8f5b-c1e4fe3d449b" />

**Screenshot 2 – Account creation completed**

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/d28c040a-9925-4108-936f-bda380126038" />

**Screenshot 3 – Script execution on the profile page**

<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/bd353c8c-67df-4bb0-b363-88934a8b88a1" />

**Screenshot 4 – Unencoded HTML in the profile source**

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/e5acce17-5c47-4530-9fed-994702b50732" />

**Screenshot 5 – XSS payload submitted as a discussion comment**

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/452b3e86-f6b9-459b-97bd-11464cf1ed37" />

**Screenshot 6 – Script execution from the saved comment**

<img width="959" height="448" alt="image" src="https://github.com/user-attachments/assets/28df3627-9e66-435a-acaa-732b72a72ac5" />

**Screenshot 7 – Stored payload in discussion data**

<img width="958" height="502" alt="image" src="https://github.com/user-attachments/assets/9471db72-6675-4766-8c61-c61cf0158c62" />

## Impact

- An attacker may execute JavaScript in the browser of a user who views the affected profile or discussion content.
- The script may expose information available to the vulnerable page or interfere with a user's interaction with the application.
- Depending on the application's functionality and the affected user's privileges, the issue may enable unauthorized actions through the user's session.

## Remediation

- Apply context-appropriate output encoding whenever profile fields and discussion comments are rendered.
- If the application intentionally permits HTML, sanitize submitted content with a maintained allowlist-based HTML sanitizer.
- Avoid inserting untrusted content directly into HTML, attributes, or executable JavaScript contexts.
- Deploy a restrictive Content Security Policy (CSP) as an additional security layer; do not rely on a WAF as the primary XSS defense.
