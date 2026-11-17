## Title 
Reflected XSS via Improper Input Filtering

## Description

**Affected URL:** https://checkout.pwnedbox.site/

**Vulnerability:** Potential Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `cat`

The application accepts user-controlled values through multiple GET parameters. During testing, the `cat` parameter was observed reflecting supplied input in the HTTP response. A crafted value containing HTML and JavaScript-related markup was used to examine how the application handles and filters user input.

The reflected content indicates that the parameter is not fully handled in a context-aware manner. However, JavaScript execution has not yet been confirmed, so this finding should be treated as reflected input / a potential XSS issue until executable behavior is demonstrated.

## Steps to Reproduce

1. Identify the available GET parameters on https://checkout.pwnedbox.site/ using x8scan.
2. Select the `cat` parameter for testing.
3. Open the following crafted URL in a browser:

   ```text
   https://checkout.pwnedbox.site/?cat=Yash%27%22%3E%3C%3E%3CSCripT%3Ea\u006cert(1)%3C/SCripT%3E
   ```

4. Allow the page to load with the crafted value supplied through the `cat` parameter.
5. Observe whether the supplied value appears in the page response.
6. Inspect the page source or HTML response and examine how the `cat` value is reflected.
7. Record whether the input is encoded, filtered, or inserted into an executable HTML/JavaScript context.

## Proof of Concept (POC)

### Screenshot 1 – GET Parameter Discovery Using x8scan

<img width="959" height="279" alt="image" src="https://github.com/user-attachments/assets/330aabe1-ba12-48e0-9e97-a3ebeb001f05" />

### Screenshot 2 – Crafted Input Submitted Through the `cat` Parameter

<img width="959" height="501" alt="image" src="https://github.com/user-attachments/assets/ff952d17-f36e-412a-9635-b99622ae6bac" />

### Screenshot 3 – Reflected Input in the Browser Response

<img width="959" height="247" alt="image" src="https://github.com/user-attachments/assets/036d7024-2ae2-4fa8-b373-5c14e49dd1ae" />

### Screenshot 4 – Reflected Input in Page Source

<img width="959" height="495" alt="image" src="https://github.com/user-attachments/assets/23a10b31-dcb4-4677-bf16-e786fe6664b8" />

**Affected Parameter:** `cat`

**Test Result:** The supplied input was observed in the response. JavaScript execution was not confirmed during this test.

## Impact

- **Input Reflection:** User-controlled data is returned in the application response.
- **HTML Injection Risk:** Improper output handling may allow injected markup to affect page content.
- **Potential XSS:** If the reflected value reaches an executable HTML or JavaScript context without encoding, it may lead to script execution.
- **Crafted-Request Abuse:** An attacker may use a specially prepared URL to deliver malicious input to visitors.

## Remediation

- **Context-Aware Output Encoding:** Encode the `cat` parameter according to the exact HTML or JavaScript context before reflecting it.
- **Input Validation:** Validate and safely handle values received through GET parameters.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML and do not rely on blacklist-based filtering as the primary defense.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
