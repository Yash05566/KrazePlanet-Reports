## Title

Stored XSS in AdPulse Network Reports

## Description

**Affected URL:** https://ads.pwnedbox.site/

The AdPulse Network Reports feature is vulnerable to Stored Cross-Site Scripting (XSS). The **Report Name** field accepts user-controlled HTML content without proper sanitization or output encoding. After a report is saved, the submitted value is stored and displayed in the reports table as HTML rather than plain text.

Consequently, the injected event handler executes JavaScript when the affected reports page is viewed. This may affect authenticated users who have access to the Network Reports section.

## Steps to Reproduce

1. Visit https://ads.pwnedbox.site/ and log in.
2. Open the **Reports** section.
3. Select **New Network Report**.
4. Enter the following payload in the **Report Name** field:

   `Yash"><><Img SrC=x OnErRoR=confirm(5)>`

5. Choose an Ad Network and complete the required report details.
6. Click **Run & Save Report**.
7. Observe the JavaScript confirmation dialog displaying `5`.
8. Close the dialog and confirm that the report has been saved.
9. Reload or revisit the Reports page. The stored payload executes when the report name is displayed.
10. Open Developer Tools and inspect the page to verify that the report name is rendered as raw HTML.

## POC (Proof of Concept)

**Screenshot 1 – Payload entered in the new network report form**

<img width="957" height="503" alt="image" src="https://github.com/user-attachments/assets/69bfa313-984d-43ff-97c7-bd9155b64af9" />

**Screenshot 2 – JavaScript confirmation triggered after saving the report**

<img width="959" height="499" alt="image" src="https://github.com/user-attachments/assets/b21b63bb-eda1-4df2-8047-54b165fbfcc7" />

**Screenshot 3 – Stored report displayed in the Network Reports table**

<img width="959" height="508" alt="image" src="https://github.com/user-attachments/assets/0053a8e4-2720-4991-b7fa-bc918eca6cc2" />

## Impact

- An attacker may execute JavaScript in the browser of authenticated users who view the affected Network Reports page.
- Depending on the application's permissions and accessible data, this could expose sensitive information or put user accounts at risk.
- The injected script may perform actions available to the affected user within the application. Session theft or account takeover was not demonstrated in the provided proof of concept.

## Remediation

- Apply context-aware output encoding when displaying report names in the Network Reports table.
- If report names are intended to support HTML, sanitize the input with a maintained allowlist-based HTML sanitizer.
- Avoid inserting untrusted values directly into HTML, attribute, or JavaScript contexts.
- Consider a restrictive Content Security Policy (CSP) as an additional layer of protection.
