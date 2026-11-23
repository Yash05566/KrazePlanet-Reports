## Title

Stored Cross-Site Scripting (XSS) in Product Reviews

## Description

**Affected URL:** https://members.pwnedbox.site/

A Stored Cross-Site Scripting (XSS) issue was found in the product review feature. Review content supplied by users is saved and later displayed without suitable output encoding. A malicious review can contain HTML with a JavaScript event handler, which runs when the stored review is rendered in the customer reviews section.

## Steps to Reproduce

1. Sign in to https://members.pwnedbox.site/
2. Visit the following product page:

   https://members.pwnedbox.site/index.php?product_id=1

3. Scroll to the **Write a Customer Review** area.
4. Choose a star rating.
5. In the **Headline / Short Title** field, submit this payload:

   ```html
   Yash"><><Img SrC=x OnErRoR=confirm('Title')>
   ```

6. In the **Detailed Review Comments** field, enter the following payload:

   ```html
   Yash"><><Img SrC=x OnErRoR=confirm('Comment')>
   ```

7. Select **Submit Product Review** to post the review.
8. Observe the JavaScript confirmation dialog triggered by the submitted review.
9. Review the page source and confirm that the submitted content appears in the customer reviews section.

## POC (Proof of Concept)

**Screenshot 1 – Review form populated with test payloads**

<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/26e9bffd-17bf-4491-8fc8-a29f39d87329" />

**Screenshot 2 – Confirmation dialog triggered by the review content**

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/9214ee5c-db6a-412a-b96c-8f7f05bec0d7" />

**Screenshot 3 – Stored payload visible in the customer review markup**

<img width="959" height="498" alt="image" src="https://github.com/user-attachments/assets/ccc5c3b2-3c6f-4ea6-8b14-8c176b390314" />

## Impact

- An attacker may execute JavaScript in the browser of users who view the affected product reviews.
- Depending on the data and functionality available to the page, the script may expose sensitive information or interfere with user activity.
- The issue may enable unauthorized actions through an affected user's session, depending on application permissions and security controls.

## Remediation

- Apply context-aware output encoding when displaying review titles and comment content.
- If HTML is intentionally supported in reviews, sanitize it using a maintained allowlist-based HTML sanitizer.
- Do not insert untrusted review content directly into HTML, attributes, or executable JavaScript contexts.
- Use a restrictive Content Security Policy (CSP) as an additional defense; do not rely on WAF filtering as the primary fix.
