# Challenge "Write_specific_byte" Writeup

- **Vulnerability**: Format String Vulnerability
  - _This vulnerability occurs when user-supplied data is used as a format string in functions like `printf`, leading to potential arbitrary memory access or modification._
- **Where**: The vulnerability exists within the `vuln` function, specifically in the `printf(buffer)` call.
  - _The `buffer` variable is filled with user input and is directly used as a format string, enabling exploitation of the format string vulnerability._
- **Impact**: The exploitation allows an attacker to manipulate the most significant byte (MSB) of a global variable `target`.
  - _By crafting a specific input, an attacker can control the MSB of `target`, altering its value to meet a predetermined condition._
- **NOTE**: This challenge demonstrates the danger of format string vulnerabilities, particularly in manipulating specific parts of memory, which can lead to unintended program behavior or security breaches.

## Steps to Reproduce

1. **Compile the Vulnerable Program**: Use the provided C code and compile it with `gcc -m32 -Wall -Wextra -ggdb -no-pie`. This sets up the program with the format string vulnerability.
2. **Determine the Address of `target`**: Use GDB or a similar debugger to find the memory address of the `target` variable. Assume the address is `0x804c044`.
3. **Craft the Payload**: Write a Python script to generate a payload that uses the format string vulnerability to modify the MSB of `target`. The payload includes the address of the MSB of `target` and uses format specifiers to adjust the number of characters printed before writing a value to the MSB with `%n`.
   ```python
   import struct

   target_address = 0x804c044  # Replace with the actual address of 'target'
   msb_address = struct.pack("<I", target_address + 3)
   num_chars_needed = 0x102 - 4  # Adjust this value as necessary
   payload = msb_address + "%{}c%7$hhn".format(num_chars_needed)

   print(payload)
    ```
4. **Run the Vulnerable Program with the Payload**: Execute the compiled program and provide the payload as input. The program should modify the MSB of `target` to `0x02`.

5. **Observe the Altered Program Behavior**: The program should acknowledge that the MSB of `target` has been successfully modified, displaying the success message and the flag.
