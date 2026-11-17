# Reflected XSS – Equifax

## Description

**Affected URL:** https://tracking.pwnedbox.site/

The `search` GET parameter is affected by a Reflected Cross-Site Scripting (XSS) vulnerability. The application returns the submitted search input in its response and also inserts that value into the JavaScript analytics-tracking function without applying the required JavaScript-context escaping.

Testing confirmed that a specially crafted search URL can escape the surrounding JavaScript string context and cause JavaScript to run in the browser. When the URL was opened, a confirmation dialog displaying `1` appeared.

## Steps to Reproduce

1. Visit the following URL in a browser:

   ```text
   https://tracking.pwnedbox.site/?search='"<!-->)</Title/</h1/</h2/</h3/</h4/</div/</p/</a/</font/</label/</button/</img/</ul/</ol/</li/</option/</span/</Style/</Script/</textArea/</iFrame/</noScript><Img Src=OnXSS OnError=(confirm)(1)>
   ```

2. Load the URL containing the crafted value.
3. Check whether a JavaScript confirmation box appears with the value `1`.
4. View the page source and find the analytics-tracking code.
5. Confirm that the supplied input is inserted into the `searchTerm` value without suitable escaping for a JavaScript string.

## POC (Proof of Concept)

**Screenshot 1 –** The Equifax tracking page before submitting the crafted search input.

<img width="1918" height="973" alt="Original Equifax search page" src="https://github.com/user-attachments/assets/20669a48-d2a1-4409-8c81-ee60d4f695be" />

**Screenshot 2 –** The confirmation dialog displayed after the crafted URL is opened.

<img width="1918" height="953" alt="JavaScript confirmation dialog" src="https://github.com/user-attachments/assets/812dbcd8-f457-49e0-aef4-1fcbe547ea44" />

**Screenshot 3 –** The page source illustrating the unescaped search input within the analytics tracking function.

<img width="1918" height="975" alt="Reflected input in analytics function" src="https://github.com/user-attachments/assets/3e84053b-4da0-4f79-95fd-51f2407efdf6" />

## Impact

- An attacker may be able to execute JavaScript in the context of a victim's browser when the victim visits a maliciously crafted link.
- Depending on the application's security controls and the data available to scripts, this could expose sensitive information or put user sessions at risk.
- The issue could potentially be abused to perform actions as the victim within the application. Account takeover or session hijacking was not independently demonstrated during this test.

## Remediation

- Apply context-aware output encoding to all untrusted values before placing them into HTML or JavaScript. For JavaScript contexts, use safe serialization rather than directly concatenating user input into a script string.
- Validate the `search` parameter on the server side, but do not rely on input filtering or tag blacklists as the primary XSS defense.
- Avoid inserting untrusted values into executable JavaScript. Prefer safe DOM APIs and pass analytics values through a structured, safely serialized mechanism.
- Deploy a restrictive Content Security Policy (CSP) as an additional defense-in-depth measure.
- A Web Application Firewall (WAF), including Cloudflare-managed rules, may help detect or block some malicious requests, but it should not replace secure output encoding and safe application design.
