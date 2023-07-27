Flag: `flag{uwu_y05_ha5_ac7ua11y_5ta7te6}`

use program:
```bash
#!/bin/bash

extract_zip() {
    local zip_file=$1
    local password_file=$2
    
    echo "Extracting: $zip_file"
    echo "Password File: $password_file"
    
    local password=$(cat "$password_file")
    local password_64=$(echo -n "$password" | base64)
    local password_32=$(echo -n "$password" | base32)
    local password_16=$(echo -n "$password" | xxd -p)
    
    # Extract the zip file using the password

    if 7z t "$zip_file" -p$password_16 > /dev/null 2>&1; then 
        7z x "$zip_file" -p$password_16; 
    fi 
    if 7z t "$zip_file" -p$password_32 > /dev/null 2>&1; then
        7z x "$zip_file" -p$password_32; 
    fi 
    if 7z t "$zip_file" -p$password_64 > /dev/null 2>&1; then
        7z x "$zip_file" -p$password_64; 
    fi 
    if 7z t "$zip_file" -p$password > /dev/null 2>&1; then
        7z x "$zip_file" -p$password; 
    fi 

    rm $zip_file $password_file

    # Find the nested zip file and password file
    local nested_zip=$(find . \( -name "*.zip" -o -name "*.7z" \) ! -name "files.zip" ! -name "chall1.7z" | head -n 1)

    echo "nested zip is: $nested_zip"
    local nested_password=$(find . -type f ! -name "*.zip" ! -name "*.7z" ! -name "enc_pass" ! -name "script.sh" | head -n 1)

    echo "nested_password is: $nested_password"

    
    if [ -n "$nested_zip" ] && [ -n "$nested_password" ]; then
        extract_zip "$nested_zip" "$nested_password"  # Recursive call
    elif [ $(find . -type f | wc -l) -eq 1 ]; then
        # Save the single file as "flag" and print its contents
        local flag_file=$(find . -type f)
        cp "$flag_file" ./flag
        echo "Flag found: $(cat ./flag)"
    fi
    
}
echo "starting now"
# Initial zip file and password file
initial_zip="files.zip"
echo "found initial_zip"
initial_password="enc_pass"
echo "found enc_pass"
extract_zip "$initial_zip" "$initial_password"

```