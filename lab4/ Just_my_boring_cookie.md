# Challenge `Just_my_boring_cookie` writeup

- **Vulnerability:** Cross-Site Scripting (XSS)
  - _The challenge exploits a Cross-Site Scripting vulnerability._
  
- **Where:** Vulnerability present in the search bar of the site.
  - _The vulnerability is found when user input in the search bar is not properly sanitized, allowing the injection of malicious scripts._

- **Impact:** Cookie theft and potential for session hijacking.
  - _Exploiting this vulnerability allows an attacker to execute arbitrary scripts in the context of the user's browser. In this case, the provided Proof of Concept (POC) script retrieves and alerts the user's cookies. An attacker could steal sensitive session information, leading to unauthorized access._

- **NOTE:** The provided POC is a JavaScript script that retrieves and alerts the user's cookies.
  - _This script demonstrates the severity of the XSS vulnerability by showing how an attacker could easily harvest sensitive information, such as session tokens, stored in the user's cookies._

## Steps to reproduce

1. Open the site in a browser.
2. Navigate to the search bar.
3. Inject the provided POC script into the search bar.
4. Execute the search.
5. Observe the alert displaying the user's cookies.

Script used: `<script> var cookies = document.cookie.split(';'); for (var i = 0; i < cookies.length; i++) { alert(cookies[i].trim()); } </script>` on a search bar of the site
