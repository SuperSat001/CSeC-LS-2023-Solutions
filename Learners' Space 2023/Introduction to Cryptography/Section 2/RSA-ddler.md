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

Flag:
`YoS{srropjskcxx}`



