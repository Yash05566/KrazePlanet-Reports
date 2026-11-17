## Title

Reflected XSS – Reddit

## Description

**Affected URL:** https://widgets.pwnedbox.site/

The `post_id` parameter in the GET request is reflected into the HTML of the page without the required attribute-level encoding. In particular, the value is placed in the `id` field of the **See More Comments** button, allowing crafted input to alter the generated markup.

During verification, the supplied test value escaped the intended attribute context and caused JavaScript to execute in the browser, confirming a Reflected XSS issue.

## Steps to Reproduce

1. First, use x8scan to inspect the target and identify parameters that may be reflected in the response.
2. Note the `post_id` parameter in the discovered parameter list.
3. Open the following test URL in a browser:

   ```text
   https://widgets.pwnedbox.site/?post_id=%22Yash%27%22%3E%3Cscript%3Ealert(1)%3C/script%3E
   ```

4. Wait for the page to finish loading and check for the JavaScript alert showing `1`.
5. Dismiss the alert and inspect the rendered page source.
6. Find the **See More Comments** button and examine its `id` attribute.
7. Verify that the parameter value has been inserted into the attribute without suitable HTML attribute encoding.

## POC (Proof of Concept)

**Screenshot 1 – ** x8scan Output Showing Discovered GET Parameters

<img width="959" height="267" alt="image" src="https://github.com/user-attachments/assets/43c72fb7-3065-4934-92ec-e6a87d3e4fa4" />

**Screenshot 2 – ** Crafted Payload and JavaScript Alert

<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/4a975805-1098-4d79-b606-d6de51990dc8" />
<img width="959" height="268" alt="image" src="https://github.com/user-attachments/assets/71487f31-666d-4785-b0a1-3886aaa7e125" />

**Screenshot 3 – ** Reflected Payload in Page Source

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/ebacfe12-0f4c-4d17-93c8-655a3f832e3a" />

## Impact

- It allows attackers to execute JavaScript in a victim's browser.
- It may expose sensitive information or put active user sessions at risk.
- It could be used to carry out unauthorized actions through the victim's browser.

## Remediation

- Encode untrusted values for the HTML attribute context before placing them in page markup.
- Validate the `post_id` value on the server and avoid building HTML attributes through direct string concatenation.
- Use safe DOM properties and APIs when updating page content dynamically.
- Add a restrictive Content Security Policy (CSP) as an extra layer of protection against script execution.
