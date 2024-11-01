# Challenge "write to memory" Writeup

- **Vulnerability**: Format String Vulnerability
  - _This vulnerability is a type of security flaw typically found in C/C++ programs that arises from the improper handling of format strings in functions like `printf`. It allows an attacker to read from or write to memory locations, potentially leading to arbitrary code execution or information disclosure._
- **Where**: The vulnerability is present in the `vuln` function, specifically in the `printf(buffer)` call.
  - _In this scenario, `buffer` is filled with user input and is used unsafely as a format string for `printf`. This enables the exploitation of the format string vulnerability._
- **Impact**: The exploitation of this vulnerability allows an attacker to write a value to a global variable `target`.
  - _By manipulating the `target` variable, the attacker can trigger a different program flow, demonstrating control over the program's execution._
- **NOTE**: This challenge demonstrates the critical importance of properly handling user input, especially in functions interpreting format strings. It also showcases the potential dangers of format string vulnerabilities leading to unexpected modifications in program behavior.

## Steps to Reproduce

1. **Compile the Vulnerable Program**: Use the provided C code and compile it with `gcc -m32 -Wall -Wextra -ggdb -no-pie`. This sets up the program with the format string vulnerability.
2. **Find the Address of `target`**: Utilize tools like GDB or readelf to find the memory address of the global variable `target`. Assume it is `0x804c040`.
3. **Craft the Payload**: Write a Python script to generate a payload that exploits the format string vulnerability. The payload should include the address of `target` and use the `%n` format specifier to write the number of printed characters so far into `target`.
   ```python
   import struct

   address = 0x804c040
   address_bytes = struct.pack("<I", address)
   X = 10  # Replace with the desired number of characters to print
   payload = address_bytes + "A" * (X - len(address_bytes)) + "%7$n"

   print(payload)
    ```
4. **Run the Vulnerable Program with the Payload**: Execute the compiled program and provide the payload as input. The program should write the number of characters printed to the `target` variable.

5. **Observe the Modified Behavior**: The program should acknowledge that the `target` variable has been modified, indicating successful exploitation of the vulnerability.
