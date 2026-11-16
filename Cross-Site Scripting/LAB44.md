## Title

Reflected XSS Through Mixed-Case Script Tag in Name Fields

## Description

**Affected URL:** https://directory.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameters:** `fname`, `lname`

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, a mixed-case script-tag payload was prepared for both parameters.

The test checks whether the supplied values are reflected into the HTML input elements without proper context-aware output encoding. The mixed-case `<SCripT>` tag is intended to check whether script-tag filtering can be bypassed and whether JavaScript executes in the browser.

## Steps to Reproduce

1. Open https://directory.pwnedbox.site/.
2. Locate the First Name and Last Name input fields.
3. Enter the following payload into the First Name field:

   ```html
   Yash'"<><SCripT>alert(1)</SCripT>
   ```

4. Enter the same payload into the Last Name field.
5. Submit the form.
6. Check whether a JavaScript alert displaying `1` appears.
7. Inspect the page source or browser Inspector.
8. Verify whether the supplied values are reflected in both `fname` and `lname` input elements without proper HTML output encoding.

## POC (Proof of Concept)

### Screenshot 1 – Payload Submitted in Both Name Fields

<img width="959" height="499" alt="image" src="https://github.com/user-attachments/assets/9a9de0da-5faf-414e-8be4-41e4cf12ab44" />

### Screenshot 2 – JavaScript Alert Execution
<img width="959" height="308" alt="image" src="https://github.com/user-attachments/assets/5ad5158d-1d86-4dd8-9120-1d47cee87ebd" />
<img width="959" height="476" alt="image" src="https://github.com/user-attachments/assets/c2bd2c46-8ab4-4e4b-a566-299f40a486d9" />


### Screenshot 3 – Reflected Payload in HTML

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/7caa055c-543b-429f-80a6-74c117cb848b" />


### HTTP Request / URL

```http
view-source:https://directory.pwnedbox.site/?fname=+Yash%27%22%3C%3E%3CSCripT%3Ealert%281%29%3C%2FSCripT%3E&lname=+Yash%27%22%3C%3E%3CSCripT%3Ealert%281%29%3C%2FSCripT%3E
```

### Payload

```html
Yash'"<><SCripT>alert(1)</SCripT>
```

**Parameters tested:**
- `fname` – First Name
- `lname` – Last Name

## Impact

- **JavaScript Execution:** A crafted URL may execute JavaScript in a visitor's browser if the input is reflected unsafely.
- **Phishing and Content Manipulation:** Injected script may alter page content or display deceptive elements.
- **Targeted Attacks:** An attacker may distribute a crafted link to users to trigger the reflected XSS when opened.

## Remediation

1. **Context-Aware Output Encoding:** HTML-attribute-encode user-controlled values before inserting them into input elements.
2. **Consistent Parameter Handling:** Apply secure validation and output encoding to both `fname` and `lname`.
3. **Safe Output Practices:** Avoid placing raw user input into HTML through unsafe string concatenation or DOM manipulation.
4. **Content Security Policy:** Implement a restrictive Content Security Policy (CSP) as an additional layer of defense.
