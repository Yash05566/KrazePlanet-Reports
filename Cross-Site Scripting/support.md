## Title

Reflected XSS in First Name and Last Name Form Fields

## Description

**Affected URL:** https://support.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameters:** `fname`, `lname`

The application accepts First Name and Last Name values through GET parameters. The lab description reports that the values are reflected into HTML input elements without proper context-aware output encoding.

## Steps to Reproduce

1. Visit https://support.pwnedbox.site/.
2. Enter the following test payload in both First Name and Last Name fields:

   ```html
   '"<><body onpageshow=alert(1)>
   ```

3. Submit the form.
4. Check whether a JavaScript alert displaying `1` appears.
5. Inspect the page source or DOM to verify reflection in both parameters.

### HTTP Request / URL

```http
https://support.pwnedbox.site/?fname=%27%22%3C%3E%3Cbody%20onpageshow%3Dalert%281%29%3E&lname=%27%22%3C%3E%3Cbody%20onpageshow%3Dalert%281%29%3E
```

### Payload

```html
'"<><body onpageshow=alert(1)>
```

## Proof of Concept (POC)

### Screenshot 1 – Payload Entered in Both Fields

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/31cc1783-ab16-4333-9f0c-ed970f760ee9" />

### Screenshot 2 – JavaScript Alert Execution

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/b80be4d9-89c7-47bc-93a8-3819c71e7d16" />

### Screenshot 3 – Reflected HTML in Both Parameters

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/3cf0dd85-f218-4607-bbbf-952ecd252e72" />


## Impact

- **Session Exposure:** Compromise active user sessions through injected scripts.
- **Account Takeover:** Potentially gain unauthorized access to user accounts.
- **Unauthorized Access:** Perform actions within the application using a victim's session.

## Remediation

- **HTML Sanitization:** Safely handle HTML tags submitted in the Report Name field.
- **Script Filtering:** Prevent execution of untrusted JavaScript code.
- **Output Encoding:** Use PHP's `htmlspecialchars()` before displaying user-controlled data.
- **WAF Implementation:** Use Cloudflare WAF as an additional layer against XSS attacks.
