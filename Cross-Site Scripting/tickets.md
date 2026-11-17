# Reflected Cross-Site Scripting (XSS) – KrazePlanet GET Parameter Reflection

## Description

**Affected URL:** https://tickets.pwnedbox.site/

**Vulnerability:** Reflected Cross-Site Scripting (XSS)

**Affected Parameters:** `fname`, `lname`

The application accepts values through the `fname` and `lname` GET parameters and reflects the supplied input in the response. The tested input was not safely handled in the page context, allowing injected JavaScript to execute in the browser when the request was submitted.

## Steps to Reproduce

1. Open https://tickets.pwnedbox.site/.
2. Locate the **First Name** and **Last Name** fields.
3. Enter the following test payload into the fields:

   ```html
   "> <body onpageshow=/*x*/this['a'+'l'+'e'+'r'+'t'](1)//>
   ```

4. Submit the form.
5. Observe a JavaScript alert displaying `1`.
6. The alert confirms that JavaScript execution was triggered through the reflected GET parameter input.

## Proof of Concept (POC)

**Tested request:**

```text
https://tickets.pwnedbox.site/?fname=%22%3E+%3Cbody+onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D%281%29%2F%2F%3E&lname=%22%3E+%3Cbody+onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D%281%29%2F%2F%3E
```

**Observed result:** A browser alert containing `1` appeared after form submission, demonstrating JavaScript execution.

### Screenshot 1 – Payload Entered in the Form

Add the first screenshot to the repository as `images/tickets-payload.png`, then uncomment the line below:

<!-- ![Payload entered in First Name and Last Name fields](images/tickets-payload.png) -->

### Screenshot 2 – JavaScript Alert

Add the second screenshot to the repository as `images/tickets-alert.png`, then uncomment the line below:

<!-- ![JavaScript alert displaying 1](images/tickets-alert.png) -->

## Impact

- **JavaScript Execution:** Trigger injected JavaScript in a visitor's browser.
- **Page Manipulation:** Potentially alter page content or display misleading elements.
- **Phishing Risk:** Use deceptive content to mislead visitors.
- **Crafted-Link Abuse:** Trigger the issue when a victim opens a specially crafted URL.

The confirmed test demonstrates JavaScript execution. Further impacts, such as session compromise or account takeover, were not tested.

## Remediation

- **Context-Aware Output Encoding:** Encode untrusted values for the exact HTML context before reflecting them in the response.
- **Safe Rendering:** Avoid inserting user-controlled input into HTML as executable markup; use safe DOM APIs such as `textContent` where applicable.
- **Server-Side Validation:** Validate and safely handle both `fname` and `lname` GET parameters.
- **Content Security Policy:** Deploy a restrictive CSP as an additional layer of protection against script execution.
