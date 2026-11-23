## Title
Stored XSS - Events & Polls Activity Center

## Description
**Affected URL:** https://articles.pwnedbox.site/

The Events & Polls section is vulnerable to Stored Cross-Site Scripting (XSS). User-submitted event and poll details are saved and displayed without proper HTML encoding. As a result, injected HTML containing JavaScript event handlers can execute when the stored content is viewed, potentially affecting other users accessing the events or polls.

## Steps to Reproduce

1. Visit https://articles.pwnedbox.site/ and sign in using the lab account.
2. Navigate to the Events & Polls section.
3. Under Create Community Event, enter the following payloads in the respective fields:
   
   `Yash"><><Img SrC=x OnErRoR=confirm(5)>`<br>
   `Yash"><><Img SrC=x OnErRoR=confirm(55)>`<br>
   `Yash"><><Img SrC=x OnErRoR=confirm(555)>`

4. Click Publish Event to submit the event.
5. Observe the JavaScript confirmation dialog and dismiss it.
6. Verify that the event is published and the submitted content appears in the scheduled events list.
7. Open the Interactive Polls section.
8. Enter similar test payloads in the Poll Question, Poll Description, and Poll Options fields.
9. Click Publish Poll and observe whether the JavaScript confirmation dialog appears.
10. Open the published event or poll and inspect its page source or DOM to verify how the submitted HTML is rendered.

## POC (Proof of Concept)

Screenshot 1: Event form populated with payloads fields.

<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/f50cb2c1-d631-495f-8974-a1b99fddef5d" />

Screenshot 2: Confirmation dialog appears after publishing the event; the event is stored and displayed.

<img width="959" height="349" alt="image" src="https://github.com/user-attachments/assets/74d988ac-4a7f-40de-97b1-7f6df20aeff1" />

Screenshot 3: Page source shows the injected payload.

<img width="959" height="499" alt="image" src="https://github.com/user-attachments/assets/e1961474-6c99-4df0-a234-25d59b662cdf" />



## Impact
- It allows attackers to execute malicious JavaScript in users' browsers through the vulnerable application.
- It may lead to session hijacking, sensitive data exposure, or account compromise.
- It could enable attackers to perform unauthorized actions using affected users' account privileges.


## Remediation
- Validate user input and sanitize unsafe HTML tags before storing or displaying content.
- Apply proper output encoding to prevent injected JavaScript from executing.
- Use PHP's htmlspecialchars() to safely render user input in HTML contexts.
- Implement CSP and WAF rules, such as Cloudflare protections, as additional security measures.
