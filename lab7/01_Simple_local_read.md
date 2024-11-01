# Challenge "Simple_local_read" Writeup

- **Vulnerability**: Format String Vulnerability
  - _This is a class of vulnerability where a function that expects a format string (like `printf`) is given a string that the user controls, allowing the user to read or manipulate memory._
- **Where**: The vulnerability is present in the `vuln` function of the provided C code, specifically in the `printf(buffer)` call.
  - _The `buffer` variable, which is filled with user input, is directly used as a format string in `printf`, without any sanitization or format specifier checks._
- **Impact**: Exploiting this vulnerability allows an attacker to read arbitrary memory locations. In this specific challenge, it can be used to leak the contents of `secret_value`, which is intended to be hidden.
  - _By carefully crafting the input (the format string), an attacker can read memory locations on the stack. This can lead to the disclosure of sensitive information, like secret keys or passwords, stored in memory._
- **NOTE**: This challenge demonstrates the dangers of using untrusted input as a format string in functions like `printf`. It highlights the need for proper input validation and sanitization in programs.

## Steps to Reproduce

1. **Compile the Vulnerable Program**: Use the provided C code and compile it with the `gcc -m32 -Wall -Wextra -ggdb -no-pie` command. This sets up the vulnerable program.
2. **Craft the Payload**: Write a Python script to generate a payload string. The payload uses format string specifiers (`%s`) to read data from the stack.
   ```python
   payload = "AAAA.%08s.%08s.%08s.%08s.%08s.%08s.%08s.%08s.%08s.%08s"
   print(payload)
    ```
3. **Run the Vulnerable Program and Send Payload**: Execute the compiled program and provide the output of the Python script as input.

4. **Observe Memory Leakage**: The program will output the contents of the stack, including the value of `secret_value`, due to the format string vulnerability being exploited.
