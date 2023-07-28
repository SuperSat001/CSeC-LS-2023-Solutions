following bash script will extract the unique ports being used in the TCP packets with names of people

```
let i=0

while [ $i -le 22 ]
do
tshark -r entities.pcap -Y "tcp.stream eq $i" -T fields -e tcp.dstport | head -n 1
((i=i+1))
done
```
run this script with the command `./extract.sh > ports.txt`

and the following python script will compute the difference between successive ports and they happen to be the ordinal of the characters of the flag

```
diff = []

with open('ports.txt', 'r') as file:
    prev_line = None  
    for line in file:
        line = line.strip()  
        if prev_line is not None:
            diff_value = int(line) - int(prev_line)  
            diff.append(diff_value)

        prev_line = line

print(diff)

for i in range(len(diff)):
    print(chr(diff[i]),end='')
print()
```
usage `python3 dec.py`
