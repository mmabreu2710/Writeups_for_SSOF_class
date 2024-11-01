# Challenge `Calling_functions` Writeup

- **Vulnerability:** Function Pointer Overwrite
  - _Example: Overwriting a function pointer in memory to redirect code execution to a different function._
- **Where:** In the `buffer[32]` and `fp` (function pointer) in the `main` function
  - _Example: The vulnerability lies in the use of `gets(buffer)` to take input, which allows overwriting adjacent memory, including the function pointer `fp`._
- **Impact:** Redirecting code execution to the `win` function
  - _Example: By overwriting `fp`, the attacker can redirect execution to the `win` function, resulting in an unintended code flow and access to the flag._
- **NOTE:** The program’s flow can be manipulated due to the unsafe use of `gets`, which does not check the size of input. The challenge is to overwrite the `fp` function pointer to point to the `win` function.

## Steps to Reproduce

1. Write a Python script to create a payload with 32 characters followed by the address of the `win` function.
2. The payload needs to fill the buffer with 32 'A's and then append the address of `win` in little-endian format.
3. Execute the Python script and pipe its output to `nc` to send it to the challenge server.
4. On execution, the `fp` function pointer is overwritten with the address of the `win` function.
5. The conditional `if(fp)` becomes true, and the program calls `fp()`, resulting in the execution of the `win` function and printing the flag.

[(POC)](function_pointer_hijack_poc.py)

### Python Payload Script (`function_pointer_hijack_poc.py`):

````python
payload = "A" * 32 + "\xf1\x86\x04\x08" # 0x80486f1 Address of win in little-endian format
print(payload)
````
````bash
python function_pointer_hijack_poc.py | nc mustard.stt.rnl.tecnico.ulisboa.pt 23153
````
