## Title

Stored XSS in the Bug Bounty Report Submission Feature

## Description

**Affected URL:** https://hackerone.pwnedbox.site/

The vulnerability is present in the vulnerability report submission feature. Content entered in the **Title**, **Description**, and **Impact** fields is stored and later displayed without proper output encoding. When a submitted report is viewed, the browser processes the injected HTML event handler, allowing JavaScript to execute. This results in Stored Cross-Site Scripting (XSS).

## Steps to Reproduce

1. Navigate to https://hackerone.pwnedbox.site/index.php?view=submit.
2. Enter the following payload in the **Title** field:

   `Yash"><><ImG SrC=x OnErRoR=confirm(1)>`

3. Enter this payload in the **Description** field:

   `Yash"><><ImG SrC=x OnErRoR=confirm(2)>`

4. Enter the following payload in the **Impact** field:

   `Yash"><><ImG SrC=x OnErRoR=confirm(3)>`

5. Choose the target program and severity, then submit the report.
6. Observe the JavaScript confirmation dialog that appears after submission.
7. Close the dialog and open the submitted report from the dashboard.
8. Check the report details and observe that the stored payload is rendered and JavaScript executes.
9. Inspect the page source to verify that the injected HTML appears in the report title, description, and impact sections without proper output encoding.

## POC (Proof of Concept)

**Screenshot 1 – Injected input in the report submission fields**

<!-- Add Screenshot 1 here: Show the Title, Description, and Impact fields containing the submitted payloads. -->

**Screenshot 2 – JavaScript confirmation displayed when viewing the report**

<!-- Add Screenshot 2 here: Show the JavaScript confirmation dialog triggered by the stored payload. -->

**Screenshot 3 – Injected content reflected in the report page source**

<!-- Add Screenshot 3 here: Show the report source where the submitted values appear in the title, description, and impact sections. -->

## Impact

- An attacker may execute JavaScript in the browser of a user who views the affected report.
- Depending on the application's permissions and accessible data, this could expose sensitive information or put user accounts at risk.
- The injected script may perform actions available to the affected user within the application. Session theft or account takeover was not demonstrated in the provided proof of concept.

## Remediation

- Apply context-aware output encoding to report titles, descriptions, and impact content whenever they are displayed.
- If report content is intended to support HTML, sanitize it with a maintained allowlist-based HTML sanitizer.
- Avoid placing untrusted input directly into HTML, attribute, or JavaScript contexts.
- Consider a restrictive Content Security Policy (CSP) as an additional layer of protection.
