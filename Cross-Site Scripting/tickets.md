# Reflected XSS through Unvalidated GET Parameters

## Description

**Affected URL:** https://tickets.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameter:** `lname`

The Tickets page reflects the value supplied through the `lname` GET parameter into its HTML response without proper context-aware output encoding. A crafted value can break out of the expected HTML attribute context and cause JavaScript to execute in the browser. During testing, the `fname` value appeared to be encoded, while the `lname` value was reflected unsafely.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   [https://tickets.pwnedbox.site/?lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E](https://tickets.pwnedbox.site/?lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E)

2. Wait for the Tickets page to load with the payload supplied through the `lname` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert popup.
5. Inspect the page source or HTML response.
6. Compare how the `fname` and `lname` parameters are reflected in the HTML.
7. Observe that `fname` is HTML-encoded, while the `lname` value is reflected without proper HTML attribute encoding.

## Proof of Concept (POC)

**Tested parameter:** `lname`

**Payload:**

```html
"> <body onpageshow=/*x*/this['a'+'l'+'e'+'r'+'t'](1)//>
```

**Observed result:** A JavaScript alert displaying `1` appears when the crafted URL is opened, confirming JavaScript execution through the `lname` parameter.

## Impact

- **JavaScript Execution:** Trigger injected scripts in a visitor's browser.
- **Content Manipulation:** Potentially modify page content or display misleading elements.
- **Phishing Risk:** Present deceptive content to visitors.
- **Crafted-Link Abuse:** Trigger the vulnerability when a victim opens a specially crafted URL.

The test confirms JavaScript execution through `lname`. Additional impacts, such as session compromise or account takeover, were not tested.

## Remediation

- **Context-Aware Output Encoding:** Encode `lname` according to its HTML attribute context before reflecting it in the response.
- **Consistent Parameter Handling:** Apply the same secure validation and output-encoding controls to `fname` and `lname`.
- **Safe Rendering:** Avoid inserting untrusted input as executable HTML; use safe DOM APIs where applicable.
- **Content Security Policy:** Implement a restrictive CSP as an additional layer of protection against script execution.
