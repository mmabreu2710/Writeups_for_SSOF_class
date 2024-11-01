# Challenge "Short_local_read" Writeup

- **Vulnerability**: Format String Vulnerability
  - _This vulnerability occurs when user-controlled input is directly used as a format string in functions like `printf`, without proper validation. This can be exploited to read or write arbitrary memory locations._
- **Where**: The vulnerability exists in the `vuln` function within the `printf(buffer)` call.
  - _The `buffer` is filled with user input and is used unsafely as a format string in `printf`. Due to the limited size of `buffer`, a different exploitation technique is required._
- **Impact**: Exploiting this vulnerability allows reading data from arbitrary memory locations. Specifically, it can be used to leak the `secret_value` string.
  - _In this scenario, the attack reveals the contents of `secret_value`, which is normally not accessible._
- **NOTE**: This challenge underscores the critical importance of handling user input safely, especially when dealing with functions that interpret format strings. The limited buffer size introduces an additional layer of complexity, requiring more sophisticated exploitation techniques.

## Steps to Reproduce

1. **Compile the Vulnerable Program**: Use the provided C code and compile it with `gcc -m32 -Wall -Wextra -ggdb -no-pie`. This will create the program with the format string vulnerability.
2. **Understand the Stack Layout**: Using a debugger like GDB, analyze the stack layout to determine the position of `secret_value`. In this case, it is found to be at the 7th position on the stack.
3. **Craft the Payload**: Write a Python script to generate a payload string that exploits the format string vulnerability. The payload uses the `%7$s` format specifier to read the string at the 7th position on the stack.
   ```python
   payload = "%7$s"
   print(payload)
    ```
4. **Execute the Vulnerable Program and Send Payload**: Run the compiled program and provide the output of the Python script as input.
5. **Observe the Output**: The program should output the content pointed to by the 7th element on the stack, revealing the `secret_value` string.
