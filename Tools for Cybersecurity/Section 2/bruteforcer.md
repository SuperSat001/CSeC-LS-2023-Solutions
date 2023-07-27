Flag: `flag{b1n42y_s3r2ch_f7w}`

```python
import subprocess
import sys

executable_path = './bruteforcer'
wordlist_path = 'wordlist.txt'

def binary_search(passwords):
    left, right = 0, len(passwords) - 1

    while left <= right:
        mid = left + (right - left) // 2
        mid_password = passwords[mid]
        print('current index = ', mid)
        # Run the bruteforcer with the mid password
        stdout, _ = run_bruteforcer(mid_password, mid)

        # Check if the mid password is correct
        if stdout[4] != 'WRONG':
            print("Correct Password:", mid_password)
            sys.exit()
        elif stdout[4] == 'WRONG' and stdout[-1] == 'high':
            right = mid - 1
        else:
            left = mid + 1

def run_bruteforcer(password, index):
      # Run the executable and set up communication with the process
    process = subprocess.Popen(executable_path, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)

    # Provide the password to the process through standard input
    input_password = password.encode('utf-8')  # Encode the password string as bytes
    stdout, stderr = process.communicate(input=input_password)

    # Check if the output indicates a correct password
    output = stdout.decode('utf-8').split()
    if len(output) >= 5 and output[4] != 'WRONG':
        print("Correct Password:", password)
        sys.exit()
        # Optionally, you can print the full output if needed
        print("Full Output:")
        print(stdout.decode('utf-8'))
    else:
        print("Incorrect Password:", password, output[-1])

    # Wait for the process to finish
    process.wait()

    # Check the return code to determine if the process succeeded or not
    if process.returncode != 0:
        print(f"Error: Execution failed with return code {process.returncode}.")


    return stdout.decode('utf-8').split(), index


with open(wordlist_path, 'r') as wordlist_file:
    passwords = sorted(wordlist_file.read().splitlines())

# Use a dictionary comprehension to create pass_dict with keys starting from 0
pass_dict = {i: password for i, password in enumerate(passwords)}

# Run the binary search
binary_search(passwords)
```

