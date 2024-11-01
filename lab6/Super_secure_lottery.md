# Challenge `Super_secure_lottery` Writeup

## Vulnerability
- **Type**: Buffer Overflow
- **Description**: The vulnerability is a classic buffer overflow, where input data exceeds the allocated buffer size.

## Where
- **Location**: `run_lottery` function in `lottery.c`
- **Specifics**: The vulnerability is present in the `run_lottery` function, specifically in the handling of the `guess` variable with the `read` function.

## Impact
- **Result**: Overwriting Adjacent Memory (Specifically, the `prize` variable)
- **Consequences**: Exploiting this vulnerability allows an attacker to overwrite the `prize` variable with the same content as the `guess` variable. This leads to an unintended match in the `memcmp` function call, effectively bypassing the intended lottery number comparison and granting access to the `getflag()` function.

## Note
- Despite the presence of stack canaries and non-executable stack (NX bit), the exploit works because it does not rely on executing code on the stack or corrupting the return address. Instead, it takes advantage of the contiguous memory layout of local variables in the `run_lottery` function.

## Steps to Reproduce

1. **Compile and Run the Program**: Compile `lottery.c` and run the resulting binary. This sets up the vulnerable environment.
2. **Input a Long String**: Enter a string longer equal to 64 characters with all the same character(e.g., 64 'a's) when prompted for a guess.
3. **Overflow Occurs**: The input overflows the `guess` buffer, which is only 8 bytes (`LOTTERY_LEN`) long, but the program reads up to 64 bytes (`GUESS_SIZE`) into it.
4. **Memory Overwrite**: The overflowed data spills into the adjacent memory location where the `prize` variable is stored, overwriting it with the same content as in `guess`.
5. **Unintended Match**: The `memcmp(prize, guess, LOTTERY_LEN)` call now compares two identical memory contents, resulting in a match.
6. **Access Granted**: The program proceeds as if the correct lottery number was guessed, leading to the execution of `getflag()` and potentially revealing sensitive information or granting unauthorized access.

[Proof of Concept Script](lottery_exploit.py)
