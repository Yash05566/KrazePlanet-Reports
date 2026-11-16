## Title

Reflected XSS Through the `returnTo` Parameter

## Description

**Affected URL:** https://go.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `returnTo`

During parameter discovery, `x8scan` identified the `returnTo` GET parameter and produced the test URL:

```text
https://go.pwnedbox.site/?returnTo=
```

The parameter is associated with the destination used by the **Cancel & return** link on the account confirmation page. The supplied test value is included in the URL below. Verify that the value is reflected into the link's `href` attribute and that the browser executes the test JavaScript before marking the issue as confirmed.

## Steps to Reproduce

1. Run parameter discovery against the target:

   ```bash
   echo "https://go.pwnedbox.site/" | x8scan
   ```

2. Note the discovered `returnTo` parameter in the transformed URL.
3. Use the following payload as the value of `returnTo`:

   ```html
   Yash'"<><SCripT>confirm("XSS");</SCripT>
   ```

4. Open the crafted URL in a browser:

   ```text
   https://go.pwnedbox.site/?returnTo=Yash%27%22%3C%3E%3CSCripT%3Econfirm%28%22XSS%22%29%3B%3C%2FSCripT%3E
   ```

5. The account confirmation page should load.
6. Check whether a JavaScript confirmation dialog appears.
7. Inspect the **Cancel & return** link and verify whether the supplied value is reflected in its `href` attribute without proper HTML attribute encoding.

## POC (Proof of Concept)

### Screenshot 1 – Parameter Discovery Using x8scan

<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/eefdf22c-1622-4e17-84db-79feb0a8ecd2" />

### Screenshot 2 – Crafted returnTo Payload in the URL

<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/43a8a270-ba93-4b69-961d-1566047648f2" />

### Screenshot 3 – JavaScript Execution

<img width="959" height="335" alt="image" src="https://github.com/user-attachments/assets/5f960be2-516b-4df9-899c-8aa13cf3338e" />

### Screenshot 4 – Reflected Value in the Cancel & return Link

<img width="959" height="499" alt="image" src="https://github.com/user-attachments/assets/3e680f3c-9041-434b-ac12-a5587c90e706" />

### Payload

```html
Yash'"<><SCripT>confirm("XSS");</SCripT>
```

### HTTP Request / URL

```http
https://go.pwnedbox.site/?returnTo=Yash%27%22%3C%3E%3CSCripT%3Econfirm%28%22XSS%22%29%3B%3C%2FSCripT%3E
```

## Impact

If the `returnTo` parameter is inserted into the page's HTML without proper context-aware encoding, a crafted URL may cause JavaScript to execute in a visitor's browser.

Potential consequences include:

- **Page Manipulation:** An attacker may alter content displayed to the visitor.
- **Phishing:** Deceptive content or forms may be presented within the trusted page.
- **Targeted Link Attacks:** An attacker may send a crafted URL to a victim and trigger the issue when it is opened.

**Evidence Status:** The provided screenshot shows the crafted `returnTo` value in the browser URL and the account confirmation page. It does not, by itself, show a JavaScript dialog or prove reflection inside the `href` attribute. Add those verification screenshots if the behavior is observed.

## Remediation

1. **Context-Aware Output Encoding:** Encode the `returnTo` value for the HTML attribute context before inserting it into the `href` attribute.
2. **URL Validation:** Accept only expected return destinations, preferably from an allowlist of trusted paths or URLs.
3. **Safe Output Handling:** Do not place raw user-controlled values into HTML attributes.
4. **Content Security Policy:** Use a restrictive Content Security Policy (CSP) as an additional defense.
