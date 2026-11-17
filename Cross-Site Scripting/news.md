## Title
Reflected XSS – HTML Tag Filter Bypass

## Description

**Affected URL:** https://news.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `title`

The application accepts user-controlled input through the `title` GET parameter and reflects it into the HTML response. A specially crafted payload can bypass the existing filtering and cause JavaScript to execute in the browser. This occurs because the supplied value is not safely encoded before being placed into the HTML document.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   ```text
   https://news.pwnedbox.site/?title=%27%22%3C!--%3E)%3C/Title/%3C/h1/%3C/h2/%3C/h3/%3C/h4/%3C/div/%3C/p/%3C/a/%3C/font/%3C/label/%3C/button/%3C/img/%3C/ul/%3C/ol/%3C/li/%3C/option/%3C/span/%3C/Style/%3C/Script/%3C/textArea/%3C/iFrame/%3C/noScript%3E%3CImg%20Src=OnXSS%20OnError=(confirm)(1)%3E
   ```

2. Allow the page to load with the crafted value supplied through the `title` parameter.
3. Observe the JavaScript confirmation dialog displaying `1`.
4. Dismiss the confirmation dialog.
5. Inspect the rendered page and its HTML source.
6. Locate the document's `<title>` element and examine how the supplied input is reflected.
7. Confirm whether the input is inserted without proper context-aware output encoding.

## Proof of Concept (POC)

### Screenshot 1 – GET Parameter Discovery

<img width="959" height="260" alt="image" src="https://github.com/user-attachments/assets/8a99da97-ca8b-40cd-9a64-c172cdca82b9" />

### Screenshot 2 – Crafted Payload in the Title Parameter

<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/330c8844-dc11-49cb-a858-71dbc5d5328e" />


### Screenshot 3 – JavaScript Confirmation Dialog

<img width="959" height="260" alt="image" src="https://github.com/user-attachments/assets/96c1a7ab-9d6e-49a0-a470-87e7103ee326" />


### Screenshot 4 – Rendered Page with Injected HTML

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/20ae8661-80bc-4450-bac8-00979e1c893f" />


### Screenshot 5 – Reflected Payload in Page Source

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/bf352531-9448-402f-8c27-23216ebfa4ed" />


**Affected Parameter:** `title`

**Payload behavior:** The supplied input escapes the expected title context, injects HTML, and triggers a JavaScript confirmation dialog.

## Impact

- **JavaScript Execution:** Run injected scripts in a visitor's browser.
- **Content Manipulation:** Modify page content or introduce misleading elements.
- **Phishing Risk:** Display deceptive content to unsuspecting visitors.
- **Crafted-Link Abuse:** Trigger the payload when a victim opens a specially crafted URL.


## Remediation

- **Context-Aware Output Encoding:** Encode the `title` parameter correctly before inserting it into the HTML document.
- **Input Validation:** Validate and safely handle user-supplied values received through GET parameters.
- **Safe HTML Handling:** Avoid relying on tag or keyword blacklists as the primary XSS defense; use safe rendering methods.
- **Content Security Policy:** Apply a restrictive CSP as an additional layer of protection against script execution.
