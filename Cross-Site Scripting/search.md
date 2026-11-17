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

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/c674c698-77b9-4177-a7eb-42f645d80321" />


### Screenshot 2 – First JavaScript Alert

<img width="940" height="217" alt="image" src="https://github.com/user-attachments/assets/2b64b699-4367-439b-bee2-681ee2fe4443" />


### Screenshot 3 – Second JavaScript Alert

<img width="940" height="258" alt="image" src="https://github.com/user-attachments/assets/4c454ddf-5bfc-4e37-bc2b-98676f0ccb4a" />


### HTTP Request / URL

```http
https://search.pwnedbox.site/?fname=yash1%27%22%3E%3Cscript%3Ealert%281%29%3C%2Fscript%3E&lname=yash2%27%22%3E%3Cscript%3Ealert%282%29%3C%2Fscript%3E
```
## Impact

- Trigger malicious scripts in users' browsers.
- Change displayed content or webpage elements.
- Trick visitors through fake or deceptive content.

## Remediation

- **Secure Output Handling:** Encode untrusted data before displaying it.
- **Parameter Checking:** Verify and filter incoming GET requests.
- **CSP Protection:** Restrict unauthorized scripts using a suitable CSP.
