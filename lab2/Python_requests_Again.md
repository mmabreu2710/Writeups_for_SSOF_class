# Challenge "Python requests Again" Writeup

## Vulnerability: Lack of Input Validation and Session Integrity

### Where: Parsing and processing server responses, and handling user input

### Impact: The code may be susceptible to unexpected server responses or manipulations in user input, leading to unintended behavior or exploitation. Additionally, there might be potential vulnerabilities related to session integrity.

**NOTE**: The provided code interacts with a web server, extracts values from its responses, and performs actions based on these values. Two functions, `get_target_value` and `get_current_value`, use regular expressions to parse specific information from the server response.

## Steps to reproduce:

1. The code establishes a session using the `requests.Session()` object to persist cookies between requests.

2. Accesses the first link (`/hello`) to set the user cookie and retrieve the target value from the server response using the `get_target_value` function.

3. Enters a loop to continuously access the second link (`/more`) until the current value matches the target value.

4. Within the loop, retrieves the current value from the server response using the `get_current_value` function.

5. Compares the current and target values. If they are equal, the loop breaks, indicating that the desired condition is met.

6. After the loop, accesses the third link (`/finish`) and prints the response.

**Observations:**

- The code relies on the assumption that the server will respond in a specific format, and any deviation might lead to extraction errors or unexpected behavior.

- Lack of input validation may make the code vulnerable to changes in the server's response structure.

- The session handling is basic, and there is no explicit validation to ensure the integrity of the session.

## Code:

```python
import requests
import re

def get_target_value(input_string):
    # Define the pattern to match "target" followed by a number
    pattern = re.compile(r'target (\d+)')

    # Search for the pattern in the input string
    match = pattern.search(input_string)

    # If a match is found, return the target value as an integer
    if match:
        target_value = int(match.group(1))
        return target_value
    else:
        # Return None if no match is found
        return None

def get_current_value(input_string):
    # Define the pattern to match "Your current value is:" followed by a number
    pattern = re.compile(r'Your current value is: (\d+)')

    # Search for the pattern in the input string
    match = pattern.search(input_string)

    # If a match is found, return the current value as an integer
    if match:
        current_value = int(match.group(1))
        return current_value
    else:
        # Return None if no match is found
        return None

link = "http://mustard.stt.rnl.tecnico.ulisboa.pt:23054"

# Create a session to persist the cookies between requests
s = requests.Session()

# Access the first link to set the user cookie
response1 = s.get(link + "/hello")
print(s.cookies)
s.cookies.set('remaining_tries', '10000')
print(response1.text)
target_value = get_target_value(response1.text)
print(target_value)

# Access the second link to get number
i = 0
while(i != -1):
   response2 = s.get(link + "/more")
   current_value = get_current_value(response2.text)
   print(response2.text)
   if (target_value == current_value):
       break;
    
response3 = s.get(link + "/finish")
print(response3.text)
