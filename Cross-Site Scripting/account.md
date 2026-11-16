## Title

Reflected XSS Through Extended Event-Handler Filtering

## Description

**Affected URL:** https://account.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Parameters:** `fname`, `lname`

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, an obfuscated JavaScript event-handler payload was submitted through both parameters.

Source inspection showed different output-handling behavior: the `fname` value was HTML-encoded, while the `lname` value was reflected as raw HTML inside its input element. The test payload constructs the `alert` function name through JavaScript string concatenation, checking whether basic function-name filtering can be bypassed.

## Steps to Reproduce

1. Open https://account.pwnedbox.site/.
2. Locate the First Name and Last Name input fields.
3. Enter the following test payload into both fields:

   ```html
   "><body onpageshow=/*x*/this['a'+'l'+'e'+'r'+'t'](1)//>
   ```

4. Submit the form.
5. Inspect the resulting page source or browser Inspector.
6. Compare the reflection of `fname` and `lname`.
7. Verify whether `fname` is HTML-encoded while the `lname` value is reflected without proper encoding.
8. Check whether the JavaScript alert executes in the browser.

## POC (Proof of Concept)

### Screenshot 1 – Payload Entered in Both Fields

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/791d5bf6-8efc-4196-9f73-548523991ead" />

### Screenshot 2 – JavaScript Alert Execution

<img width="959" height="490" alt="image" src="https://github.com/user-attachments/assets/cc4b1884-84d8-416c-95ce-ee125db8abb8" />



### HTTP Request / URL

```http
https://account.pwnedbox.site/?fname=%22%3E%3Cbody+onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D%281%29%2F%2F%3E&lname=
```

### Payload

```html
"><body onpageshow=/*x*/this['a'+'l'+'e'+'r'+'t'](1)//>
```

**Parameters tested:**
- `fname` – First Name (reported as HTML-encoded)
- `lname` – Last Name (reported as reflected without proper encoding)

## Impact

If an attacker shares a specially prepared link with a visitor, the unescaped `lname` value may cause script code to run in the visitor's browser. Depending on the features available on the page, this could be used to:

- **Mislead Visitors:** Change page elements or display content that appears to come from the legitimate website.
- **Abuse the User Session:** Attempt actions available to the visitor while they are signed in.
- **Deliver Phishing Content:** Display fake messages or forms to trick visitors into sharing information.

The different handling of `fname` and `lname` also shows that output encoding is not applied consistently across the form.

## Remediation

1. **Encode Before Displaying:** Escape both name values for the HTML attribute context whenever they are placed into input fields.
2. **Use One Secure Output Method:** Handle `fname` and `lname` through the same trusted output-encoding process so one field cannot remain unprotected.
3. **Avoid Raw HTML Insertion:** Do not build HTML attributes by directly joining user-supplied strings. Use safe templating or DOM APIs.
4. **Add a CSP:** Configure a restrictive Content Security Policy to reduce the impact if an injection issue is missed.
