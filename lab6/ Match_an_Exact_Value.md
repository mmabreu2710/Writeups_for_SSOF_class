# Challenge `Match_an_Exact_Value` Writeup

- **Vulnerability:** Buffer Overflow
  - _Example: Buffer Overflow, where user input overflows the buffer size, allowing memory manipulation._
- **Where:** In the `buffer[64]` within the `main` function
  - _Example: The vulnerability is in the main function, specifically at `gets(buffer)`, which does not check the size of the input._
- **Impact:** Overwriting the `test` variable to a specific value (`0x61626364`)
  - _Example: Allows manipulation of the `test` variable's value to `0x61626364`, enabling the user to receive the congratulations message and the flag._
- **NOTE:** The use of `gets` function, which is known for its lack of input size validation, allows buffer overflow. The challenge hints at using ASCII values to achieve the specific value in the `test` variable.

## Steps to Reproduce

1. Write a Python script to create a payload that first fills the buffer and then overwrites the `test` variable.
2. The payload should have 64 'A's to fill up the buffer, followed by the ASCII values of 'd', 'c', 'b', 'a' (`\x64\x63\x62\x61`) in reverse order due to little-endian format.
3. Execute the Python script and pipe its output to `nc` to send it to the challenge server.
4. This will overflow the `buffer` and correctly set the `test` variable to `0x61626364`.
5. The program then executes the code within the `if (test == 0x61626364)` block, leading to the congratulations message and the flag being printed.

[(POC)](buffer_overflow_specific_value_poc.py)

### Python Payload Script (`buffer_overflow_specific_value_poc.py`):

````python
payload = "A" * 64 + "\x64\x63\x62\x61"
print(payload)
````
###Command to Send Solution:
````bash
python buffer_overflow_specific_value_poc.py | nc mustard.stt.rnl.tecnico.ulisboa.pt 23152
````
