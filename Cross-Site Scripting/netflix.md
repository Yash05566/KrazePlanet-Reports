## Title

Stored XSS in Username and Movie Reaction Features

## Description

**Affected URL:** https://netflix.pwnedbox.site/

The movie reaction feature is vulnerable to Stored Cross-Site Scripting (XSS). User-submitted comments are stored and later displayed in the Viewer Reactions section without proper HTML output encoding.

As a result, injected HTML containing JavaScript event handlers is interpreted by the browser when the movie details page is loaded. This allows malicious JavaScript to execute in the browser of users who view the affected movie's reactions.

## Steps to Reproduce

1. Sign in and open a movie details page, for example: `https://netflix.pwnedbox.site/105`.
2. Scroll to the **Share Your Thoughts & Reactions** section.
3. Enter the following payload in the comment field:

   `Yash"><><Img SrC=x OnErRoR=confirm('Comment')>`

4. Submit the reaction.
5. Observe the JavaScript alert displaying `Comment`.
6. Close the alert.
7. Inspect the movie details page source and locate the saved comment in the viewer reactions section.
8. Verify that the comment is rendered without proper HTML encoding.

## POC (Proof of Concept)

**Screenshot 1 – XSS Payload Entered in Movie Reaction Form**

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/3ce81988-e026-4106-b490-c23b0114b5b9" />

**Screenshot 2 – JavaScript Execution Triggered After Reaction Submission**

<img width="959" height="508" alt="image" src="https://github.com/user-attachments/assets/2043c57b-53b1-41ee-8c01-4b209002e472" />

**Screenshot 3 – Stored Comment Reflected in Viewer Reactions Section**

<img width="959" height="499" alt="image" src="https://github.com/user-attachments/assets/e1a0ac14-5c58-4233-b3b0-b04cb0858329" />

## Impact

- An attacker can execute malicious JavaScript in the browser of users who view the affected movie's reaction section.
- The vulnerability may expose sensitive information accessible within the application's context and put user accounts at risk.
- Malicious scripts could potentially perform unauthorized actions using the privileges of affected users.

## Remediation

- Implement proper HTML output encoding when displaying user-submitted movie reaction comments.
- If HTML content is supported, use a trusted allowlist-based HTML sanitizer to remove unsafe tags and event-handler attributes.
- Apply a restrictive Content Security Policy (CSP) to provide an additional layer of protection against script execution.
