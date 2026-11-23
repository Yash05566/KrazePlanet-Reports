## Title
Stored XSS - Events & Polls Activity Center

## Description
**Affected URL:** https://articles.pwnedbox.site/

The Events & Polls Activity Center contains a stored cross-site scripting (XSS) vulnerability. User-controlled event and poll fields are stored and later rendered without proper HTML output encoding. HTML event-handler payloads execute JavaScript when the created event or poll is displayed. This can affect other users who view the stored content.

## Steps to Reproduce

1. Open `https://articles.pwnedbox.site/` and log in to the lab account.
2. Navigate to the **Events & Polls** section.
3. In the **Create Community Event** form, enter the following payloads into the Event Title, Event Date & Time, and Event Description fields:
   
   `Priyansh"><><Img SrC=x OnErRoR=confirm(1)>`<br>
   `Priyansh"><><Img SrC=x OnErRoR=confirm(2)>`<br>
   `Priyansh"><><Img SrC=x OnErRoR=confirm(3)>`

5. Click Publish Event.
6. Observe the JavaScript confirmation dialog and dismiss it.
7. Verify that the event is saved and the injected content appears in the scheduled event listing.
8. Open Interactive Polls and enter similar payloads in the Poll Question, Poll Description, and Poll Options fields.
9. Click Publish Poll and observe the confirmation dialog.
10. View the stored event or poll and inspect the page source to verify that the injected HTML is rendered.

## POC (Proof of Concept)

Screenshot 1: Event form populated with payloads in the title, date/time, and description fields.

<img width="1918" height="953" alt="image" src="https://github.com/user-attachments/assets/b637bcac-c982-44e2-84ff-0a8140ff4fac" />

Screenshot 2: Confirmation dialog appears after publishing the event; the event is stored and displayed.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/f13b0c9d-3220-4c6b-8517-1e6599550dc2" />

Screenshot 3: Page source shows the injected payload in the stored event title, date/time, and description.

<img width="1918" height="955" alt="image" src="https://github.com/user-attachments/assets/41aee1e9-f0ab-4626-93d5-3feb0c83aaa8" />

Screenshot 4: Poll form populated with payloads in the question, description, and options.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/2fea5476-8819-4499-9248-556524f45509" />

Screenshot 5: Confirmation dialog appears after publishing the poll.

<img width="1918" height="972" alt="image" src="https://github.com/user-attachments/assets/9d882c38-8afb-4c02-a032-a404b504dde7" />

Screenshot 6: Page source shows the stored event content rendered with injected markup.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/de16c02d-69b8-441e-bb2a-532309d6b511" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server