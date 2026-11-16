## Title

Reflected XSS in GET Parameter Reflection Form

## Description

**Affected URL:** https://feedback.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameters:** `fname`, `lname`

The page accepts First Name and Last Name values through GET parameters. Testing showed that script code placed in these fields is executed when the submitted request is loaded. This indicates that the reflected values are not being safely encoded before they reach the browser.

## Steps to Reproduce

1. Open https://feedback.pwnedbox.site/.
2. Enter the following value in the **First Name** field:

   ```html
   "><sCripT>alert(1)</sCripT>
   ```

3. Enter the following value in the **Last Name** field:

   ```html
   "><sCripT>alert(2)</sCripT>
   ```

4. Submit the form.
5. Observe the JavaScript alert popups. The test displays `1` and `2` in the browser.

## POC (Proof of Concept)

### Screenshot 1 – Form Fields with Test Payloads

<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/918d97bc-4584-42a8-9e41-8a8ec59bc2d5" />


### Screenshot 2 – JavaScript Alert

<img width="940" height="213" alt="image" src="https://github.com/user-attachments/assets/501626f9-ff96-4bd2-b84c-f8afbe20d5a7" />


### Screenshot 3 – Second JavaScript Alert

<img width="940" height="361" alt="image" src="https://github.com/user-attachments/assets/62dbcbcc-4bbd-4dcd-ad1d-3dce315c5e4a" />


### Payloads Used

**First Name (`fname`):**

```html
"><sCripT>alert(1)</sCripT>
```

**Last Name (`lname`):**

```html
"><sCripT>alert(2)</sCripT>
```

### HTTP Request / URL

```http
https://feedback.pwnedbox.site/?fname=%22%3E%3CsCript%3Ealert%281%29%3C%2FsCript%3E&lname=%22%3E%3CsCript%3Ealert%282%29%3C%2FsCript%3E
```

## Impact

A specially prepared link may cause JavaScript to run when someone opens the page with the crafted `fname` and `lname` values. Depending on the application's features and the user's session, this could be used to:

- Change or add misleading content to the page.
- Send the visitor to an unwanted website.
- Display a fake form to request information.
- Attempt actions available to the user's active session.

**Actual Impact:** The supplied test scripts executed in the browser and produced alert popups showing `1` and `2`. No account compromise or data access was demonstrated.

**Potentially Affected Users:** Visitors who open a crafted URL containing the affected GET parameters.

## Remediation

- Encode `fname` and `lname` for the correct HTML output context before displaying them.
- Treat values received through GET parameters as untrusted input.
- Avoid building page content by inserting raw input into HTML.
- Use safe DOM methods and add a restrictive Content Security Policy (CSP) as an additional safeguard.
