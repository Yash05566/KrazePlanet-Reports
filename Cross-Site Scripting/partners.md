## Title

Blind Stored XSS in the Informatica Cloud Registration Form

## Description

**Affected URL:** https://partners.pwnedbox.site/index.php?view=register  

The Informatica Cloud application is vulnerable to Stored Cross-Site Scripting (XSS) through the **Company Name** field in the account registration form. The application accepts and stores HTML containing an event handler without properly sanitizing the input or encoding it when displayed.

When the saved Company Name value is rendered on the user's profile page, the browser interprets the injected HTML and executes the JavaScript. This behavior was confirmed by the browser prompt appearing after registration.

## Steps to Reproduce

1. Open the registration page at https://partners.pwnedbox.site/index.php?view=register.
2. Complete the required account registration fields.
3. Enter the following payload in the **Company Name** field:

   ```html
   <ImG src=x onerror=prompt(5)>
   ```

4. Submit the registration form.
5. After the profile page loads, observe the JavaScript prompt displaying `5`.


## POC (Proof of Concept)

**Screenshot 1 – Registration form with the Company Name payload**

<img width="959" height="508" alt="image" src="https://github.com/user-attachments/assets/853f8c03-ad70-4f28-8cb8-549b5b733d6d" />

**Screenshot 2 – Browser prompt triggered on the profile page**

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/29868150-cb0d-4158-bcb5-6d5d64cdd0f6" />


## Impact

- An attacker may cause JavaScript to execute in the browser of users who view a page displaying the stored Company Name value.
- The injected content could be used to manipulate page content or present phishing content.
- Depending on application permissions and protections, the script may perform unauthorized actions or access information available to the affected user.

## Remediation

- Apply context-aware output encoding to the Company Name value wherever it is displayed.
- If HTML is not required in this field, treat the value as plain text and reject or safely encode markup.
- Validate the Company Name input on the server side and avoid rendering untrusted event-handler attributes.
- Deploy a restrictive Content Security Policy (CSP) as an additional layer of protection.
