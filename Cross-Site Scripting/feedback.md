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

### Screenshot 1 – JavaScript Alert

<!-- Add Screenshot 1 Here -->

### Screenshot 2 – Form Fields with Test Payloads

<!-- Add Screenshot 2 Here -->

### Screenshot 3 – Second JavaScript Alert

<!-- Add Screenshot 3 Here -->

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
