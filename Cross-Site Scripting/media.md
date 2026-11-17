## Title 
Reflected XSS via Username GET Parameter

## Description

**Affected URL:** https://media.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `username`

The application accepts a username through the `username` GET parameter. The supplied test URL contains HTML and JavaScript markup in this parameter, which should be treated as untrusted input. If the application reflects this value into the page without context-aware output encoding, it may allow script execution in a visitor's browser.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   ```text
   https://media.pwnedbox.site/?username=Yash%27%22%3E%3Cscript>alert(1)</script>
   ```

2. Allow the page to load with the supplied value in the `username` parameter.
3. Check whether a JavaScript alert displaying `1` appears.
4. If the alert appears, dismiss it and inspect the page source or HTML response.
5. Locate where the `username` value is reflected, including the profile link's `href` attribute.
6. Verify whether the reflected value is properly encoded for that HTML context.

**Test Parameter:** `username`

**Payload represented in the URL:**

```html
Yash'"><script>alert(1)</script>
```


## Proof of Concept (POC)

### Screenshot 1 – x8scan output showing the transformed URL and username parameter.

<img width="959" height="254" alt="image" src="https://github.com/user-attachments/assets/bda55b9d-5ee5-4196-850f-ecfaa69beab9" />


### Screenshot 2 – page displaying the reflected username.

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/1cedbf7f-c059-433d-8d4b-2a3074287e91" />


### Screenshot 3 – JavaScript alert displaying 1, confirming successful execution.

<img width="959" height="266" alt="image" src="https://github.com/user-attachments/assets/c360be35-d41d-4333-a038-f901848c9f52" />


## Impact

- **JavaScript Execution:** If the payload executes, an attacker may run scripts in a visitor's browser.
- **Content Manipulation:** Injected scripts may alter displayed page content.
- **Phishing Risk:** Malicious page elements could be used to mislead visitors.
- **Crafted-Link Abuse:** A specially prepared URL could be sent to a victim to trigger the issue.

## Remediation

- **Context-Aware Output Encoding:** Encode the `username` value for its exact HTML context before reflecting it in the response.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML; use safe DOM APIs such as `textContent` where appropriate.
- **Input Validation:** Validate and safely handle values received through the `username` GET parameter.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
