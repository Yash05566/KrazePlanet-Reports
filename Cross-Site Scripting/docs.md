## Title

Reflected XSS in Page Heading

## Description

**Affected URL:** https://docs.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `q`

The page accepts a value through the `q` GET parameter. During testing, a crafted value was reflected into the page and caused JavaScript to run in the browser. This shows that the reflected input is not being safely handled for its output context.

## Steps to Reproduce

1. Open https://docs.pwnedbox.site/.
2. Add the test payload to the `q` parameter in the URL.
3. Load the modified URL in the browser.
4. Observe the JavaScript confirmation dialog displaying `1`.
5. Dismiss the dialog and check the page, where the reflected content is visible.

## POC (Proof of Concept)

### Screenshot 1 – Payload in the `q` Parameter

<!-- Add Screenshot 1 Here -->

### Screenshot 2 – JavaScript Execution

<!-- Add Screenshot 2 Here -->

### Payload Used

```html
</h1>'"1<!--></Title/</Textarea/</Script/></Iframe><Details/Open/OnToggle%3D(confirm)(1)-->
```

### HTTP Request / URL

```http
https://docs.pwnedbox.site/?q=%3C%2Fh1%3E%27%221%3C!--%3E%3C%2FTitle%3E%3C%2FTextarea%3E%3C%2FScript%3E%3C%2FIframe%3E%3CDetails%2FOpen%2FOnToggle%3D(confirm)(1)--%3E
```

## Impact

A crafted URL may cause the supplied JavaScript to execute when another visitor opens it. Depending on the content and functions available on the page, this could be used to:

- Insert misleading content into the page.
- Redirect a visitor to another website.
- Display a fake prompt or form.
- Attempt actions available through the visitor's active session.

**Actual Impact:** The supplied payload executed successfully and displayed a browser dialog containing `1`. No account access, data theft, or other follow-on impact was demonstrated.

**Potentially Affected Users:** Visitors who open a crafted link containing the vulnerable `q` parameter.

## Remediation

- Encode the `q` parameter value for the correct HTML output context before displaying it.
- Treat URL parameters as untrusted input.
- Avoid inserting raw user-controlled values into HTML.
- Use safe DOM APIs and apply a restrictive Content Security Policy (CSP) as an additional safeguard.
