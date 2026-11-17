# Reflected XSS – Reddit

## Description

**Affected URL:** https://widgets.pwnedbox.site/

The `post_id` parameter in the GET request is reflected into the HTML of the page without the required attribute-level encoding. In particular, the value is placed in the `id` field of the **See More Comments** button, allowing crafted input to alter the generated markup.

During verification, the supplied test value escaped the intended attribute context and caused JavaScript to execute in the browser, confirming a Reflected XSS issue.

## Steps to Reproduce

1. First, use x8scan to inspect the target and identify parameters that may be reflected in the response.
2. Note the `post_id` parameter in the discovered parameter list.
3. Open the following test URL in a browser:

   ```text
   https://widgets.pwnedbox.site/?post_id=%22Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C/script%3E
   ```

4. Wait for the page to finish loading and check for the JavaScript alert showing `1`.
5. Dismiss the alert and inspect the rendered page source.
6. Find the **See More Comments** button and examine its `id` attribute.
7. Verify that the parameter value has been inserted into the attribute without suitable HTML attribute encoding.

## POC (Proof of Concept)

**Screenshot 1 – Parameter testing:** The browser address bar contains the modified `post_id` request used to test the page.

<img width="1900" height="266" alt="Modified request used for testing" src="https://github.com/user-attachments/assets/9ed9d004-0d97-41e5-8e74-8d6e9e15af94" />

**Screenshot 2 – Execution evidence:** The browser displays the JavaScript dialog after processing the crafted input.

<img width="1912" height="972" alt="Browser dialog confirming script execution" src="https://github.com/user-attachments/assets/85c88da2-60c4-4af3-8ad7-d1d3964ff316" />

**Screenshot 3 – Reflection location:** The HTML source reveals the user-supplied value within the button markup, specifically its `id` attribute.

<img width="1918" height="967" alt="Source inspection of reflected button attribute" src="https://github.com/user-attachments/assets/e602f2fb-30a4-46fe-92a7-64319628ce4e" />

## Impact

- It allows attackers to execute JavaScript in a victim's browser.
- It may expose sensitive information or put active user sessions at risk.
- It could be used to carry out unauthorized actions through the victim's browser.

## Remediation

- Encode untrusted values for the HTML attribute context before placing them in page markup.
- Validate the `post_id` value on the server and avoid building HTML attributes through direct string concatenation.
- Use safe DOM properties and APIs when updating page content dynamically.
- Add a restrictive Content Security Policy (CSP) as an extra layer of protection against script execution.
