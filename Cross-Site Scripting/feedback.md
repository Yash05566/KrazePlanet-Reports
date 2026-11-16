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
   "><sCript>alert(1)</sCript>
   ```

3. Enter the following value in the **Last Name** field:

   ```html
   "><sCript>alert(2)</sCript>
   ```

4. Submit the form.
5. Observe the JavaScript alert popups. The test displays `1` and `2` in the browser.

## POC (Proof of Concept)

### Screenshot 1 – Form Fields with Test Payloads

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/28ed7d14-ec94-4400-9e14-345d6557b0f4" />


### Screenshot 2 – JavaScript Alert

<img width="940" height="217" alt="image" src="https://github.com/user-attachments/assets/9ff27834-5775-489c-aac5-0edb21175f3e" />


### Screenshot 3 – Second JavaScript Alert

<img width="940" height="258" alt="image" src="https://github.com/user-attachments/assets/c51f5aba-b4f9-477d-ba05-134313e87e31" />


### Payloads Used

**First Name (`fname`):**

```html
"><sCript>alert(1)</sCript>
```

**Last Name (`lname`):**

```html
"><sCript>alert(2)</sCript>
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
