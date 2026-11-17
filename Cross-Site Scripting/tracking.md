## Title

Reflected XSS – Equifax

## Description

**Affected URL:** https://tracking.pwnedbox.site/

The `search` GET parameter is affected by a Reflected Cross-Site Scripting (XSS) vulnerability. The application returns the submitted search input in its response and also inserts that value into the JavaScript analytics-tracking function without applying the required JavaScript-context escaping.

Testing confirmed that a specially crafted search URL can escape the surrounding JavaScript string context and cause JavaScript to run in the browser. When the URL was opened, a confirmation dialog displaying `1` appeared.

## Steps to Reproduce

1. Visit the following URL in a browser:

   ```text
   https://tracking.pwnedbox.site/?search=Yash%27%22%3C!--%3E)%3C/Title/%3C/h1/%3C/h2/%3C/h3/%3C/h4/%3C/div/%3C/p/%3C/a/%3C/font/%3C/label/%3C/button/%3C/img/%3C/ul/%3C/ol/%3C/li/%3C/option/%3C/span/%3C/Style/%3C/Script/%3C/textArea/%3C/iFrame/%3C/noScript%3E%3CImg%20Src=OnXSS%20OnError=(confirm)(1)%3E
   ```

2. Load the URL containing the crafted value.
3. Check whether a JavaScript confirmation box appears with the value `1`.
4. View the page source and find the analytics-tracking code.
5. Confirm that the supplied input is inserted into the `searchTerm` value without suitable escaping for a JavaScript string.

## POC (Proof of Concept)

**Screenshot 1 –** The Equifax tracking page before submitting the crafted search input.

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/3a671600-c4e9-4f56-9a63-b10867ce5b7f" />

**Screenshot 2 –** Crafted Payload and JavaScript Alert

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/2d9120ee-dc06-4829-8b78-a97fbe598d1e" />
<img width="959" height="212" alt="image" src="https://github.com/user-attachments/assets/4ddb5e9a-ad71-4d36-8fbe-26270f640098" />

**Screenshot 3 –** The page source illustrating the unescaped search input within the analytics tracking function.

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/5779c5e1-79a3-4c22-b681-459a3d85cc31" />

## Impact

- An attacker may be able to execute JavaScript in the context of a victim's browser when the victim visits a maliciously crafted link.
- It allows attackers to hijack user sessions.
- It allows attackers to perform unauthorized actions within the application.

## Remediation

- Validate and properly encode user input before displaying it on the page.
- Avoid directly inserting user input into JavaScript code.
- Deploy a restrictive Content Security Policy (CSP) as an additional defense-in-depth measure.
- A Web Application Firewall (WAF), including Cloudflare-managed rules, may help detect or block some malicious requests, but it should not replace secure output encoding and safe application design.
