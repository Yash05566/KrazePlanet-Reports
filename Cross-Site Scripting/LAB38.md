## Title

Reflected XSS Through Cookbook Name Fields

## Description

**Affected URL:** https://cookbook.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameters:** `fname`, `lname`

The page accepts First Name and Last Name values through GET parameters. During testing, SVG event-handler payloads entered in these fields were executed by the browser after the form was submitted. This indicates that the submitted values are reflected without appropriate output encoding.

## Steps to Reproduce

1. Open https://cookbook.pwnedbox.site/.
2. In the **First Name** field, enter:

   ```html
   "><svg onload=alert(1)>
   ```

3. In the **Last Name** field, enter:

   ```html
   "><svg onload=alert(2)>
   ```

4. Click **Submit**.
5. Observe the first JavaScript alert displaying `1`.
6. Dismiss the first alert and observe the second alert displaying `2`.

## POC (Proof of Concept)

### Screenshot 1 – Payloads Entered in the Form

<img width="940" height="494" alt="image" src="https://github.com/user-attachments/assets/397588a9-0739-4a0c-81be-c1c5943e7b5d" />

### Screenshot 2 – First Alert Popup

<img width="940" height="354" alt="image" src="https://github.com/user-attachments/assets/11767b2b-af93-4657-a07b-f5ac2c4bfb28" />

### Screenshot 3 – Second Alert Popup

<img width="940" height="356" alt="image" src="https://github.com/user-attachments/assets/17904c72-c663-43cb-8e59-8f0a67423340" />

### Payloads Used

**First Name (`fname`):**

```html
"><svg onload=alert(1)>
```

**Last Name (`lname`):**

```html
"><svg onload=alert(2)>
```

### HTTP Request / URL

```http
https://cookbook.pwnedbox.site/?fname=%22%3E%3Csvg+onload%3Dalert%281%29%3E&lname=%22%3E%3Csvg+onload%3Dalert%282%29%3E
```

## Impact

A crafted link containing these parameter values could trigger JavaScript when another visitor opens it. Depending on the site's functionality and the visitor's session, this type of issue may be used to:

- Change or insert misleading content into the page.
- Redirect a visitor to an external website.
- Display a fake form to collect information.
- Attempt actions available to the visitor's active session.

**Actual Impact:** Both SVG event-handler payloads executed successfully. The browser displayed separate alert popups containing `1` and `2`. No account compromise or data access was demonstrated.

**Potentially Affected Users:** Visitors who open a crafted URL containing the affected `fname` and `lname` parameters.

## Remediation

- Encode `fname` and `lname` using the correct output context before displaying them.
- Treat all values received through GET parameters as untrusted.
- Avoid inserting raw user input into HTML; use safe DOM methods.
- Add a restrictive Content Security Policy (CSP) as an additional layer of protection.
