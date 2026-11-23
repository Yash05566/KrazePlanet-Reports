## Title

Stored XSS in Username and Movie Reaction Features

## Description

**Affected URL:** https://netflix.pwnedbox.site/

The Netflix clone contains Stored Cross-Site Scripting (XSS) issues in two areas that accept user-controlled content. The username submitted during registration is saved and later displayed in the navigation bar without proper HTML encoding. Similarly, movie reaction comments are stored and rendered unsafely in the viewer reactions section.

When these stored values are displayed, the browser processes the injected HTML event handlers and executes JavaScript. This can affect users who view the navigation bar or movie details containing the submitted content.

## Steps to Reproduce

### A. Stored XSS in Username

1. Visit `https://netflix.pwnedbox.site/index.php`.
2. Open the **Sign Up** form.
3. Enter the following payload in the **Username** field:

   `Yash"><><Img SrC=x OnErRoR=confirm(12)>`

4. Complete the remaining registration fields and submit the form.
5. Observe the JavaScript alert displaying `12`.
6. Close the alert.
7. Inspect the page source and find the username displayed in the navigation bar.
8. Confirm that the submitted username is included in the HTML without proper output encoding.

### B. Stored XSS in Movie Reaction Comment

1. Sign in and open a movie details page, for example: `https://netflix.pwnedbox.site/101`.
2. Scroll to the **Share Your Thoughts & Reactions** section.
3. Enter the following payload in the comment field:

   `Yash"><><Img SrC=x OnErRoR=confirm('Comment')>`

4. Submit the reaction.
5. Observe the JavaScript alert displaying `Comment`.
6. Close the alert.
7. Inspect the movie details page source and locate the saved comment in the viewer reactions section.
8. Verify that the comment is rendered without proper HTML encoding.

## POC (Proof of Concept)

**Screenshot 1 – Registration form with the supplied username input**

<!-- Add Screenshot 1 here: Show the registration form with the injected payload in the Full Name and Username fields. -->

**Screenshot 2 – Alert triggered by the stored username after registration**

<!-- Add Screenshot 2 here: Show the JavaScript alert displaying 12 after registration. -->

**Screenshot 3 – Username reflected in the navigation bar HTML**

<!-- Add Screenshot 3 here: Show the page source where the username appears in the navigation bar without proper encoding. -->

**Screenshot 4 – Movie reaction form containing the test comment**

<!-- Add Screenshot 4 here: Show the comment field with the injected payload before submission. -->

**Screenshot 5 – Alert displayed after submitting the movie reaction**

<!-- Add Screenshot 5 here: Show the JavaScript alert displaying Comment. -->

**Screenshot 6 – Stored reaction visible in the page source**

<!-- Add Screenshot 6 here: Show the viewer reactions section in the page source with the submitted comment rendered without proper encoding. -->

## Impact

- An attacker may execute JavaScript in the browser of users who view the affected username or movie reaction.
- Depending on the application's permissions and accessible data, this could expose sensitive information or put user accounts at risk.
- The injected script may perform actions available to the affected user within the application. Session theft or account takeover was not demonstrated in the provided proof of concept.

## Remediation

- Apply context-aware output encoding when displaying usernames and movie reaction comments.
- If the application intentionally supports HTML in comments, sanitize it with a maintained allowlist-based HTML sanitizer.
- Avoid inserting untrusted input directly into HTML, attribute, or JavaScript contexts.
- Consider a restrictive Content Security Policy (CSP) as an additional layer of protection.
