```python
text = '000000011101010100010001010101000101000111010001110101000000010001010101110001000101110100011101011101110000000111010111010001110111011100011101010001000000010100010101000000010111000111010000000100011101000101000111011101000111011100010111'
# convert text to morse code
def convert_to_morse(text):
    output = ''
    for i in text:
        if i == '0':
            output += '.'
        else:
            output += '='
    return output
  
morse_text = convert_to_morse(text)
print(morse_text)
```
https://cryptii.com/pipes/morse-code-to-text

Flag: `_behind_every_code_is_an_enigma`