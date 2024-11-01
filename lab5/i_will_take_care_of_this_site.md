# Challenge: i_will_take_care_of_this_site

- **Vulnerability**: SQL Injection
  - _Eg, A malicious SQL query can manipulate the backend database through the login form._
- **Where**: The vulnerability is present in the login page.
  - _Eg, User authentication form where users input their username and password._
- **Impact**: Exploiting this vulnerability allows unauthorized access.
  - _Eg, The attacker can bypass authentication and gain access to restricted areas of the website or user accounts._

- **NOTE**: This type of vulnerability often occurs when user inputs are not properly sanitized or validated, allowing an attacker to inject SQL commands that are executed by the database.

## Steps to reproduce

1. Navigate to the login page of the site.
2. In the username field, enter `admin' OR 1=1 --`.
3. Leave the password field blank or enter any random text.
4. Submit the form.
5. The SQL query behind the login form becomes `SELECT * FROM users WHERE username = 'admin' OR 1=1 --' AND password = 'user_input_password'`.
6. Since `1=1` is always true, and the rest of the query is commented out, the database returns a valid user, bypassing the authentication check.
7. Now, unauthorized access is granted.
8. Check his profile for flag.

[(POC)](`sql_injection_login_poc.py`)
