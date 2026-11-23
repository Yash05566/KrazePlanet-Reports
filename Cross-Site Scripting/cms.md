## Title
Stored XSS - Shopify

## Description
**Affected URL:** https://cms.pwnedbox.site/

The Quill-based CMS allows users to publish articles containing HTML. The editor indicates that submitted content is stored without modification and later displayed through `innerHTML` on the articles page. A JavaScript payload placed in an article is saved and executes when the published articles are viewed, demonstrating a Stored Cross-Site Scripting (XSS) vulnerability that can affect other users.

## Steps to Reproduce

1. Sign in to the CMS at `https://cms.pwnedbox.site/`.
2. Open the **Write** section and start creating an article.
3. Add the following test payload to the article title or HTML content field:

   `Yash"><><Img SrC=x OnErRoR=confirm(1)>`

4. Publish the article.
5. Open the **My Articles** page.
6. Observe the JavaScript confirmation dialog triggered by the injected content.
7. Close the dialog and inspect the published article with browser developer tools.
8. Confirm that the submitted payload is retained and rendered in the article's DOM.

## POC (Proof of Concept)

**Screenshot 1 – Editor behavior and HTML rendering notice**

<!-- Add Screenshot 1 here: Quill editor showing the HTML/source editing option and the notice about rendering content with innerHTML. -->

**Screenshot 2 – Article fields containing the test input**

<!-- Add Screenshot 2 here: Article title, description, or tags fields with the Yash payload entered before publishing. -->

**Screenshot 3 – JavaScript execution after publication**

<!-- Add Screenshot 3 here: Confirmation dialog displayed after opening the published article. -->

**Screenshot 4 – Stored input visible in the article DOM**

<!-- Add Screenshot 4 here: Developer tools showing the published article and the stored payload in its DOM. -->

## Impact
- It can execute attacker-controlled JavaScript in the browser of users who view the affected article.
- It may expose sensitive information available to the application and put user sessions at risk.
- It may allow unauthorized actions to be performed through an affected user's account, depending on the account's privileges.

## Remediation
- Sanitize permitted HTML with a reputable HTML sanitizer and validate content before storing or publishing it.
- Avoid rendering untrusted article content with `innerHTML`; use safe DOM APIs and context-aware output encoding where HTML is not required.
- Apply a restrictive Content Security Policy (CSP) as an additional layer of protection against script execution.
