# Challenge "PwnTools Sockets" Writeup

## Vulnerability: Lack of Robustness in Response Parsing

### Where: Parsing server responses and decision-making based on extracted values

### Impact: The code may be susceptible to unexpected server responses or manipulations in input, leading to unintended behavior or exploitation.

**NOTE**: The provided code interacts with a remote server, extracts values from its responses, and performs actions based on these values. Two functions, `extract_values` and `extract_values2`, parse specific information from the server response.

## Steps to reproduce:

1. The code establishes a connection to a remote server using the `remote` function from the `pwn` library.

2. Receives the initial response from the server and prints it.

3. Extracts values from the initial response using the `extract_values` function.

4. Checks if the extraction was successful. If not, prints an error message and exits.

5. If extraction is successful, compares the current and target values. If they are not equal, sends 'MORE' to the server; otherwise, sends 'FINISH' and prints the final response.

6. Enters a loop to continuously receive and process responses from the server.

7. In each iteration, extracts the current value using the `extract_values2` function.

8. Checks if the extraction was successful. If not, prints an error message and exits the loop.

9. Compares the current value with the target. If they are not equal, sends 'MORE' to the server; otherwise, sends 'FINISH' and prints the final response.

10. Finally, closes the connection to the remote server.

**Observations:**

- The code relies on the assumption that the server will respond in a specific format, and any deviation might lead to extraction errors or unexpected behavior.

- Lack of robustness in response parsing may make the code vulnerable to changes in the server's response structure.

- The code does not handle exceptions or errors gracefully, and improvements can be made for better error handling.

## Code:

```python
from pwn import *

def extract_values(input_string):
    lines = input_string.split('\n')
    
    if len(lines) < 3:
        return None, None

    current_line = lines[3]
    target_line = lines[1]

    current = int(current_line.split('=')[1].strip()[:-1])
    target = int(''.join(filter(str.isdigit, target_line.split()[-1])))

    return current, target

def extract_values2(input_string):
    lines = input_string.split('\n')
    
    if len(lines) < 2:
        return None

    try:
        current_line = lines[2]
        current = int(current_line.split('=')[1].strip()[:-1])
        return current
    except IndexError:
        return None

# Connect to the server
conn = remote('mustard.stt.rnl.tecnico.ulisboa.pt', 23055)

response = conn.recv().decode().strip()
print(response)

current, target = extract_values(response)

if current is None or target is None:
    print("Failed to extract values. Exiting.")

if current != target:
    conn.sendline('MORE')
else:
    conn.sendline('FINISH')
    print(conn.recvline().decode().strip())  # Print the final response

try:
    while True:
        response = conn.recv().decode().strip()
        print(response)

        current = extract_values2(response)

        if current is None:
            print("Failed to extract current value. Exiting.")
            break

        if current != target:
            conn.sendline('MORE')
        else:
            conn.sendline('FINISH')
            print(conn.recvline().decode().strip())  # Print the final response
            break

finally:
    # Close the connection
    conn.close()
