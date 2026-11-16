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


### Payload Used

```html
<script>alert(1)</script>
```

### HTTP Request / URL

```http
https://freshcart.pwnedbox.site/index.php?q=%3Cscript%3Ealert%281%29%3C%2Fscript%3E
```

## Impact

A crafted search link could cause the supplied JavaScript to run when another user opens it. Depending on the pages and actions available to that user, this type of issue may be misused to:

- Show altered or misleading information on the page.
- Send the user to an external website.
- Present a fake sign-in prompt.
- Attempt actions using the user's current session.

**Observed Result:** The test payload executed and displayed an alert containing `1`. No account access, data theft, or other impact was demonstrated during this test.

**Potentially Affected Users:** Visitors who open a crafted URL containing the payload in the `q` parameter.

## Remediation

- Encode search terms before inserting them into the HTML response.
- Treat all search input as untrusted, even when it comes from a URL.
- Use safe DOM APIs and avoid placing raw user input into HTML.
- Add a Content Security Policy (CSP) as a secondary protection.
