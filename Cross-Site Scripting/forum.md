## Title
Stored XSS - Acronis

## Description
**Affected URL:** https://forum.pwnedbox.site/

A Stored Cross-Site Scripting (XSS) issue was found in the Profile Signature feature of the Acronis Forum. Signature content provided by a user is saved and later displayed as HTML in forum posts without adequate sanitization. By placing HTML with a JavaScript event handler in the signature, an attacker can cause script execution when other forum members view a post displaying that signature. The script runs in the security context of the forum.

## Steps to Reproduce

1. Visit `https://forum.pwnedbox.site/` and sign in.
2. Go to **My Profile** and open **Edit Profile**.
3. Find the **Profile Signature** field.
4. Enter the following test payload:

   `Yash"><><Img SrC=x OnErRoR=confirm(111)>`

5. Select **Save Profile** to save the signature.
6. Visit a forum thread or post where the signature appears.
7. Notice the JavaScript confirmation dialog.
8. Inspect the page source or DOM and confirm that the saved signature is rendered as HTML.

## POC (Proof of Concept)

**Screenshot 1 – Profile fields populated with the test payload**

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/487de314-bfe5-4d0a-8142-92a2ca8000f2" />

**Screenshot 2 – Saved profile and JavaScript confirmation**

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/39aec5d4-f391-4680-a46e-b44a57ec375e" />

**Screenshot 3 – Stored signature visible in the page DOM**

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/700a4e99-fbf3-4e36-9a9b-c5a11f4b2324" />

## Impact
- It can execute attacker-controlled JavaScript in the browser of users viewing a post with the malicious signature.
- It may put user sessions at risk, expose application-accessible information, or potentially result in account compromise.
- It could let an attacker perform unauthorized actions through affected users' accounts, including changing forum data or misusing their permissions.

## Remediation
- Sanitize user input and block unsafe HTML tags and JavaScript event handlers.
- Use proper output encoding before displaying user-submitted content.
- Apply PHP htmlspecialchars() to safely render user input.
- Add a restrictive Content Security Policy (CSP) and use WAF protections, such as Cloudflare rules, as supplementary defenses rather than relying on them as the primary fix.
