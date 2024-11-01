# Challenge "Write Specific Value" Writeup

- **Vulnerability**: Format String Vulnerability
  - _This type of vulnerability occurs when input provided by a user is used directly as a format string in functions like `printf` without proper validation or sanitization. This can lead to arbitrary memory access or modification._
- **Where**: The vulnerability is present in the `vuln` function, particularly in the `printf(buffer)` call.
  - _Here, `buffer` is filled with user input and directly used in `printf`, leading to a potential format string attack._
- **Impact**: This vulnerability allows an attacker to modify the value of a global variable (`target`) to a specific value (in this case, 327).
  - _By carefully crafting the input, an attacker can control the number of characters printed and use the `%n` format specifier to write this count into the `target` variable._
- **NOTE**: This challenge highlights the risks associated with unsanitized format strings and demonstrates how they can be exploited to alter program execution flow or manipulate application data.

## Steps to Reproduce

1. **Compile the Vulnerable Program**: Use the provided C code and compile it with `gcc -m32 -Wall -Wextra -ggdb -no-pie`. This prepares the program with the format string vulnerability.
2. **Find the Address of `target`**: Use debugging tools like GDB to find the memory address of the `target` variable, which is found to be `0x804c040`.
3. **Craft the Payload**: Write a Python script to create a payload that utilizes the format string vulnerability. The payload includes the address of `target` and uses format specifiers to adjust the number of characters printed to 327 before writing this value into `target` with `%n`.
   ```python
   import struct

   target_address = 0x804c040
   address_bytes = struct.pack("<I", target_address)
   num_chars_needed = 327 - 4  # Account for the 4 bytes of the address
   payload = address_bytes + "%{}c%7$n".format(num_chars_needed)

   print(payload)
    ```
4. **Run the Vulnerable Program with the Payload**: Execute the compiled program and input the generated payload. The program should modify the `target` variable to the value 327.

5. **Observe the Altered Program Behavior**: The program acknowledges that the `target` variable has been changed, indicating successful exploitation of the vulnerability.
