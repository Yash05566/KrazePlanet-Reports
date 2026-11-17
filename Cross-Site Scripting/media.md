# Reflected XSS via Username GET Parameter – Imgur Mobile

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

## Proof of Concept (POC)

### Screenshot 1 – Crafted URL Containing the Username Payload

![Crafted URL with the username parameter](images/media-crafted-url.png)

**Test Parameter:** `username`

**Payload represented in the URL:**

```html
Yash'"><script>alert(1)</script>
```

**Evidence note:** The supplied screenshot shows the crafted URL and payload in the browser address bar. It does not, by itself, demonstrate the alert execution or unsafe reflection in the page source. Add screenshots of the alert and the relevant HTML/source reflection after confirming those results.

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
