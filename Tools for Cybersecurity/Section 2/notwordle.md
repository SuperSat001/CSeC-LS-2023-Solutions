Flag: `flag{pr377y_5u23_7h15_15_n07_w0rd13}`

```python 
import subprocess

executable_path = './notwordle'

def wordle(password):
    process = subprocess.Popen(executable_path, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    input_password = password.encode('utf-8')
    stdout, stderr = process.communicate(input=input_password)
    process.wait()

    if process.returncode != 0:
        print(f"Error: Execution failed with return code {process.returncode}.")
    return stdout.decode('utf-8').split()[-5]

good_string = ''
matches = 0    
for i in range(30):   
    initial_chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_'
    for char in initial_chars:
        password = ''
        password = good_string + (char * (30 - i))
        try: 
            matched_chars = int(wordle(password))
        except:
            print(good_string + char)
        # print(matched_chars)
        if matched_chars > matches:
            matches += 1
            good_string += char
            break
```

