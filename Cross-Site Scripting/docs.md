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

### Payload Used

```html
</h1>'"1<!--></Title/</Textarea/</Script/></Iframe><Details/Open/OnToggle%3D(confirm)(1)-->
```

## POC (Proof of Concept)

### Screenshot 1 – Payload in the `q` Parameter

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/dd8e1090-2807-42a3-b33c-7835327b6ae3" />

### Screenshot 2 – JavaScript Execution

<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/73def5fb-3fed-4cab-84f3-4055d78c5973" />


## Impact

- Insert misleading content into the page.
- Run injected scripts in a victim's browser.
- Display a fake prompt or form.

## Remediation

- Encode the `q` parameter value for the correct HTML output context before displaying it.
- Treat URL parameters as untrusted input.
- Avoid inserting raw user-controlled values into HTML.
