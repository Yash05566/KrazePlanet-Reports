## Title

Reflected XSS Through the `returnTo` Parameter

## Description

**Affected URL:** https://go.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `returnTo`

During parameter discovery, `x8scan` identified the `returnTo` GET parameter and produced the test URL:

```text
https://go.pwnedbox.site/?returnTo=x8scan1
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

<!-- Add the x8scan output screenshot here. -->

### Screenshot 2 – Crafted returnTo Payload in the URL

<!-- Add the screenshot showing the account confirmation page and crafted URL here. -->

### Screenshot 3 – JavaScript Execution

<!-- Add a screenshot of the confirmation dialog only if it appears during testing. -->

### Screenshot 4 – Reflected Value in the Cancel & return Link

<!-- Add page-source or Inspector evidence showing the href attribute. -->

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
