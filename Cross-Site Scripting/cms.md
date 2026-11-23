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

**Screenshot 1 – HTML Editor and Article fields containing the test input**

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/71de80e2-9776-4ea1-b660-55d70e7acea9" />

**Screenshot 2 – JavaScript execution after publication**

<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/268f4444-a3af-46fd-ac4b-d3e99a6ab2b6" />

**Screenshot 3 – Stored input visible in the article DOM**

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/b791a72f-ea5a-41ad-9dc7-30c1e07a039e" />

## Impact
- It can execute attacker-controlled JavaScript in the browser of users who view the affected article.
- It may expose sensitive information available to the application and put user sessions at risk.
- It may allow unauthorized actions to be performed through an affected user's account, depending on the account's privileges.

## Remediation
- Properly encode user-supplied data before rendering it, and sanitize HTML using a trusted HTML sanitizer when HTML content is permitted.
- Apply server-side input validation and prevent unsafe HTML elements such as <script>, <img>, and <svg> from being rendered.
- Configure a Web Application Firewall (WAF), such as Cloudflare, with XSS protection rules as an additional defense-in-depth measure.
