## Title
 
Reflected XSS – Category Filter

## Description

**Affected URL:** https://shop.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `sort`

The shop application reflects the value supplied through the `sort` GET parameter in the sorting label and category navigation links. During testing, the reflected input was not properly encoded for its HTML context. A crafted payload containing a mixed-case script tag and a Unicode escape sequence caused JavaScript to execute in the browser, demonstrating that the sorting parameter can be abused through a specially crafted URL.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   ```text
   https://shop.pwnedbox.site/?category=all&sort=Yash%27%22%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E
   ```

2. Allow the shop page to load with the crafted value supplied through the `sort` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert dialog.
5. Inspect the page source or HTML response.
6. Locate the sorting label and category navigation links.
7. Verify how the supplied `sort` value is reflected in those elements.


**Affected Parameter:** `sort`

**Test Payload:**

```html
Yash'"><ScRipT>a\u006cert(1)</ScRipT>
```

## Proof of Concept (POC)

### Screenshot 1 – Shop Page Showing Category and Sorting Options

<img width="956" height="506" alt="image" src="https://github.com/user-attachments/assets/64f98501-c6a3-4aa3-be83-5c4e6e372e00" />

### Screenshot 2 – Crafted Payload and JavaScript Alert

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/3abfd146-2984-4d4f-813b-8cc9532679ad" />
<img width="959" height="299" alt="image" src="https://github.com/user-attachments/assets/21166438-5cfc-4279-9470-24a490260a4d" />


### Screenshot 3 – Reflected Payload in Page Source

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/344fd2d3-1840-41c9-95bf-f114ec9e978f" />


## Impact

- **Session Hijacking Risk:** Injected scripts may expose or misuse session-related information, depending on cookie protections.
- **Account Compromise:** Successful exploitation could increase the risk of unauthorized access to user accounts.
- **Unauthorized Actions:** Scripts may perform actions through a victim's active session, subject to application controls.

## Remediation

- **Context-Aware Output Encoding:** Encode the `sort` value correctly before reflecting it in the sorting label and category navigation links.
- **Input Validation:** Validate and safely handle values received through the `sort` GET parameter.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML; do not rely on filtering individual tags or JavaScript function names as the primary defense.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
