## Title

Blind Stored XSS in the Informatica Cloud Registration Form

## Description

**Affected URL:** https://partners.pwnedbox.site/index.php?view=register  
**HTTP Method:** POST  
**Affected Parameter:** Company Name

The Informatica Cloud application is vulnerable to Stored Cross-Site Scripting (XSS) through the **Company Name** field in the account registration form. The application accepts and stores HTML containing an event handler without properly sanitizing the input or encoding it when displayed.

When the saved Company Name value is rendered on the user's profile page, the browser interprets the injected HTML and executes the JavaScript. This behavior was confirmed by the browser prompt appearing after registration.

## Steps to Reproduce

1. Open the registration page at https://partners.pwnedbox.site/index.php?view=register.
2. Complete the required account registration fields.
3. Enter the following payload in the **Company Name** field:

   ```html
   <img src=x onerror=prompt(2)>
   ```

4. Submit the registration form.
5. After the profile page loads, observe the JavaScript prompt displaying `2`.

## POC (Proof of Concept)

**Lab Address:** https://partners.pwnedbox.site  
**HTTP Method:** POST  
**Vulnerable Parameter:** Company Name

**Payload:**

```html
<img src=x onerror=prompt(2)>
```

**Request Parameter:**

```http
Company Name=<img src=x onerror=prompt(2)>
```

**Observed Result:**

The application saves the supplied value from the Company Name field. When the profile page displays this stored value, the browser interprets the HTML and runs the injected JavaScript, showing a prompt containing `2`. The profile also displays the value as a broken image, indicating that the submitted HTML is being interpreted.

**Screenshot Evidence:**

**Screenshot 1 – Registration form with the Company Name payload**

<img width="1912" height="1038" alt="image" src="https://github.com/user-attachments/assets/ab50fc2c-338f-4a79-974d-a0b8c01b5531" />

**Screenshot 2 – Browser prompt triggered on the profile page**

<img width="1919" height="981" alt="image" src="https://github.com/user-attachments/assets/ea774b6d-d68e-4c77-880f-94847001ec34" />

**Screenshot 3 – Profile page showing the stored registration value**

<img width="1919" height="972" alt="image" src="https://github.com/user-attachments/assets/262b52e9-e48e-4edd-ad00-fd1578cb9963" />

## Impact

- An attacker may cause JavaScript to execute in the browser of users who view a page displaying the stored Company Name value.
- The injected content could be used to manipulate page content or present phishing content.
- Depending on application permissions and protections, the script may perform unauthorized actions or access information available to the affected user.

## Remediation

1. Apply context-aware output encoding to the Company Name value wherever it is displayed.
2. If HTML is not required in this field, treat the value as plain text and reject or safely encode markup.
3. Validate the Company Name input on the server side and avoid rendering untrusted event-handler attributes.
4. Deploy a restrictive Content Security Policy (CSP) as an additional layer of protection.
