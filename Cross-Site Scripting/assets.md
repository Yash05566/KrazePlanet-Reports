## Title

Reflected XSS in PUBG Community Filter

## Description

**Affected URL:** https://assets.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `p`

The PUBG Community page uses the `p` GET parameter for its filter value. During testing, the supplied input was reflected in the page's `data-query` attribute without suitable HTML attribute encoding. A crafted value was able to break out of the attribute context and execute JavaScript in the browser.

## Steps to Reproduce

1. Open https://assets.pwnedbox.site/.
2. Locate the **Filter** field on the Community page.
3. Enter the following test value:

   ```html
   Yash'"><SCripT>confirm("XSS");</SCripT>
   ```

4. Click **Search**.
5. Observe the confirmation dialog displaying `XSS`.

### Payload Used

```html
Yash'"><SCripT>confirm("XSS");</SCripT>
```

### HTTP Request / URL

```http
https://assets.pwnedbox.site/?p=Yash%27%22%3CScRipT%3Econfirm%28%22XSS%22%29%3B%3C%2FScRipT%3E
```


## POC (Proof of Concept)

### Screenshot 1 – Payload Entered in the Community Filter

<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/96919fd5-4e23-4129-8d77-0bc6fc7d5136" />

### Screenshot 2 – JavaScript Confirmation Dialog

<img width="959" height="397" alt="image" src="https://github.com/user-attachments/assets/57f8abfe-40f3-4470-b901-214958f8b8e2" />


## Impact

- Change or inject content displayed on the page.
- Show misleading messages or fake interface elements.
- Increase the risk of unauthorized account access.
- Attempt actions available to the visitor's active session.


## Remediation

- Apply context-aware HTML attribute encoding to the `p` parameter before placing it in the `data-query` attribute.
- Treat all query-string values as untrusted input.
- Avoid inserting raw user-controlled values into HTML attributes or page content.
- Use safe DOM APIs and implement a restrictive Content Security Policy (CSP) as an additional layer of protection.
