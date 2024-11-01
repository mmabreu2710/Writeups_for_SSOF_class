# Challenge `Simple_Overflow` Writeup

- **Vulnerability:** Buffer Overflow
  - _Example: Buffer Overflow, where user input exceeds the buffer size and overwrites adjacent memory._
- **Where:** In the `buffer[128]` within the `main` function
  - _Example: The buffer overflow vulnerability is present in the main function where `gets(buffer)` is used to take user input without size check._
- **Impact:** Overwriting the `test` variable to a non-zero value, resulting in unintended code execution
  - _Example: Allows altering the control flow of the program by changing the `test` variable's value, enabling the user to receive the "YOU WIN!" message and the flag._
- **NOTE:** The `gets` function is used, which is unsafe due to lack of buffer size check. This allows an attacker to input more characters than the buffer size, leading to memory corruption.

## Steps to Reproduce

1. Write a Python script that creates a payload with a string longer than 128 characters.
2. The payload should be composed of 129 'A's to overflow the buffer and change the `test` variable's value in the stack.
3. Execute the Python script and pipe its output to `nc` to send it to the challenge server.
4. The overflow of `buffer` with 129 'A's alters the adjacent `test` variable, setting it to a non-zero value.
5. The program then executes the code within the `if(test != 0)` block, resulting in the "YOU WIN!" message and the flag being printed.

[(POC)](buffer_overflow_basic_poc.py)

### Python Payload Script (`buffer_overflow_basic_poc.py`):

````python
payload = "A" * 129
print(payload)
````

### Command to Send Solution:

````bash
python buffer_overflow_basic_poc.py | nc mustard.stt.rnl.tecnico.ulisboa.pt 23151
````
