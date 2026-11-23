## Title

Stored XSS in the Support Center Ticket Form

## Description

**Affected URL:** https://digitalocean.pwnedbox.site/

The Support Center ticket form is affected by Stored Cross-Site Scripting (XSS). Input submitted through the **Subject** and **Description** fields is saved and later rendered in the Support Tickets listing without adequate output encoding. As a result, the browser interprets the submitted HTML instead of treating it as text. The issue was confirmed by submitting a ticket and triggering a JavaScript confirmation dialog when the stored description was displayed.

## Steps to Reproduce

1. Visit https://digitalocean.pwnedbox.site/.
2. Open **Support Center**, then choose **Create Ticket**.
3. Enter the following payload in the **Subject** field:

   `Yash"'><><Img SrC=x OnErRoR=confirm(5)>`

4. Add this payload to the **Description** field:

   `Yash"'><><Img SrC=x OnErRoR=confirm(55)>`

5. Choose a priority and submit the ticket.
6. Once the ticket is submitted, observe the JavaScript confirmation dialog showing `2`.
7. Close the dialog and locate the newly created ticket in the Support Tickets listing.
8. Inspect the page source to confirm that the submitted subject and description appear in the stored ticket content.

## POC (Proof of Concept)

**Screenshot 1 – Payloads submitted through the ticket form**

<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/e042149f-55d3-4195-81af-00145ee58613" />

**Screenshot 2 – Stored description triggers a JavaScript dialog**

<img width="959" height="398" alt="image" src="https://github.com/user-attachments/assets/e3433f57-be38-47f8-9ccd-44e8a33f5a25" />

**Screenshot 3 – Submitted values visible in the ticket page source**

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/39dd60f4-e836-4cc2-a75d-558668f5c4ef" />

## Impact

- An attacker may execute JavaScript in the browser of a user who views the affected ticket.
- Depending on the application's permissions and accessible data, this could expose sensitive information or put user accounts at risk.
- The script may perform actions available to the affected user in the application. These outcomes were not demonstrated in the provided proof of concept.

## Remediation

- Apply context-appropriate output encoding whenever ticket subjects and descriptions are rendered.
- If the application intentionally supports HTML in ticket content, sanitize it with a well-maintained allowlist-based HTML sanitizer before storage or display.
- Avoid inserting untrusted values directly into HTML, attributes, or JavaScript contexts.
- Consider a restrictive Content Security Policy (CSP) as an additional defense-in-depth measure.
