```bash
#!/bin/bash

extract_zip() {
    local zip_file=$1
    echo "Extracting: $zip_file"
    7z x "$zip_file" ;
    rm $zip_file 

    # Find the nested zip file and password file
    local nested_zip=$(find . \( -name "*.zip" -o -name "*.7z" \) ! -name "files.zip" ! -name "chall1.7z" | head -n 1)

    echo "nested zip is: $nested_zip"

    if [ -n "$nested_zip" ]; then
        extract_zip "$nested_zip"   # Recursive call
    elif [ $(find . -type f | wc -l) -eq 1 ]; then
        # Save the single file as "flag" and print its contents
        local flag_file=$(find . -type f)
        cp "$flag_file" ./flag
        echo "Flag found: $(cat ./flag)"
    fi
}
echo "starting now"
# Initial zip file and password file
initial_zip="Riddle.zip"
echo "found initial_zip"

extract_zip "$initial_zip" 
```

Output: `++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>++++..----.-.++++++.+++++++++++.-------------------.+++.+++++.>----------.<-----.>----.<-.>---.<-------------------.>--------.-..++.-------.<.>++++++++++++++++.+++++.-------.---------.<+++.>-----.++++++++++++++++..--.<---.>+++.---------------.+++.+.+++++++++++.-----------------.++++++++++++++++.<.>----------.+.--.------.+++++++++++++++++.----------.+++++++++++.-----------.++++++++++.------------.+++++.----------.+.++++++++++++++.-----.---.++.----.+++++++++++++.<++++++++++++++++++.>.<+++++++++.++++.<++++++++++++++++++++.>-----.--..++++.---------.++++.>.<++.+.-.>++++.<-----.>----.<+++++.>---.<.++++.-----.>-.<+.>++++.<.>--.-.<+.--.>+++.<----.>.<+++++++.>---.<<.>>++++.<---.>-.<-.>--.<----.>.-.<++++++.--.>+++++++.<-.---.++++++.>-------.<<.>>++++.<-.++++.-------.--.>.-----.<++++++.<.>--.>++++++++.<-------.>----.<+++++.>-.<<.>++++.--.<+++.>---.-.>++.-.---.<-.+++++++.>+++++++.<-.>-----.++.----.+++++++.----.<-.>++++.<+++.-------.++.>----.<<---.>>--.<++.<+++.>----.>+.<++++++++.>--..+++.<--.>.<---.---.>---.---.<++++.<++++++++.`

Output using brainfck: `JJFEKVCFKZFVES2KJJLE2UZSJ5EUUS2VGJKVEU2KLJDUKVKUINDESNKMIVDVMQ2LJJNEIVKLKZFVKSKOJRKVKTSLJVFVMS2WJVITETSKIZHEKS2WJNGEWRK2IZBVGU2KI5FEWVSDKZJUWSZVIZLEGV2TI5EUMSSVKVHESPI=`


Base32 Decode 4 times:
`srropjskcxx`

Now Decode using Vigenere cipher with key = "KEY" (https://www.dcode.fr/vigenere-cipher) to get
`intelligent`
as the zip password.

Use this hint to get all the `d`/`e` pairs. You will not be able to determine the `0th` pair.

```Suppose the index of the private key is i, then it has been calculated by solving the congruence
(e**i)*d==1 modulo phi where e,d,phi are the ith elements of their list respectively.
example: d[2] would be calculated as (e[2]**2)*d[2]==1 modulo phi[2] and so on.```

Now, start reversing the algo using standard analysis (XOR reversible etc) by nothing that the first 4 characters of the flag are know - `YoS{`

Flag : YoS{cr7pt0_1$_s0_mU3h_fUn_r1ght}



