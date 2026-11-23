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

   `Yash"><><Img SrC=x OnErRoR=confirm(3)>`

5. Select **Save Profile** to save the signature.
6. Visit a forum thread or post where the signature appears.
7. Notice the JavaScript confirmation dialog.
8. Inspect the page source or DOM and confirm that the saved signature is rendered as HTML.

## POC (Proof of Concept)

**Screenshot 1 – Profile fields populated with the test payload**

<img width="1891" height="967" alt="Profile fields containing the XSS test input" src="https://github.com/user-attachments/assets/048354ad-0822-43f2-81c4-23deb7138c46" />

**Screenshot 2 – Saved profile and JavaScript confirmation**

<img width="1918" height="962" alt="Profile save confirmation and JavaScript dialog" src="https://github.com/user-attachments/assets/b1940a35-cb20-4f8b-9a94-20294ba6ca3b" />

**Screenshot 3 – Stored signature visible in the page DOM**

<img width="1918" height="960" alt="DOM showing the stored profile signature" src="https://github.com/user-attachments/assets/fce872bb-eef6-44ea-81be-0d60a142805c" />

## Impact
- It can execute attacker-controlled JavaScript in the browser of users viewing a post with the malicious signature.
- It may put user sessions at risk, expose application-accessible information, or potentially result in account compromise.
- It could let an attacker perform unauthorized actions through affected users' accounts, including changing forum data or misusing their permissions.

## Remediation
- Sanitize allowed signature HTML and reject unsafe elements or event-handler attributes before storing or displaying it.
- Encode user-controlled content for its output context and avoid rendering untrusted data as raw HTML.
- In PHP applications, use context-appropriate escaping such as `htmlspecialchars()` when outputting text into HTML.
- Add a restrictive Content Security Policy (CSP) and use WAF protections, such as Cloudflare rules, as supplementary defenses rather than relying on them as the primary fix.
