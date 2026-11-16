## Title

Reflected XSS Through First Name and Last Name Parameters

## Description

**Affected URL:** https://support.pwnedbox.site/

**Vulnerability Type:** Reflected Cross-Site Scripting (XSS)

**Affected Parameters:** `fname`, `lname`

The page accepts First Name and Last Name values through GET parameters. During testing, the supplied values were entered into both fields using an HTML event-handler test string.

The test payload is:

```html
'"<><body onpageshow=alert(1)>
```

The intended test is to determine whether these values are reflected into the page's HTML without context-aware output encoding, allowing browser-side JavaScript execution. The uploaded screenshot shows the payload entered in both fields before submission.

## Steps to Reproduce

1. Open https://support.pwnedbox.site/.
2. Locate the **First Name** and **Last Name** fields.
3. Enter the following test payload in the First Name field:

   ```html
   '"<><body onpageshow=alert(1)>
   ```

4. Enter the same payload in the Last Name field.
5. Submit the form.
6. Check whether a JavaScript alert displaying `1` appears.
7. Inspect the resulting page source or DOM and verify whether the values are reflected in the `fname` and `lname` input elements without proper HTML attribute encoding.

## Proof of Concept (POC)

### Screenshot 1 – Payload Entered in Both Form Fields

The screenshot supplied for this report shows the test payload entered in both the First Name and Last Name fields before form submission.

<!-- Add the post-submission alert screenshot here, if observed. -->

<!-- Add page-source or DOM evidence showing reflection in both parameters here. -->

### Payload

```html
'"<><body onpageshow=alert(1)>
```

### Parameters

```text
fname
lname
```

### Evidence Note

The supplied screenshot confirms that both fields contain the test payload. It does not show the post-submission alert or the resulting HTML/DOM reflection. Add those screenshots after confirming the behavior; do not report JavaScript execution as verified until it is observed.

## Impact

If the application reflects these values into its response without suitable output encoding, an attacker may be able to:

- **Execute JavaScript in a Visitor's Browser:** A crafted URL may trigger script execution when opened by a victim.
- **Manipulate Page Content:** Injected script may alter visible page elements.
- **Conduct Targeted Attacks:** A malicious link may be sent to users to trigger the issue when they visit the page.

## Remediation

1. **Context-Aware Output Encoding:** HTML-attribute-encode user-controlled values before placing them in input element attributes.
2. **Validate Both Parameters:** Apply consistent handling to `fname` and `lname`; validation should not replace output encoding.
3. **Use Safe Rendering Practices:** Avoid inserting untrusted input into HTML through unsafe string concatenation or DOM APIs.
4. **Content Security Policy:** Deploy a restrictive Content Security Policy (CSP) as an additional layer of protection.
