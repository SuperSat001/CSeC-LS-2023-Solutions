Extract individual frames from both the gifs using Python.

```python
from PIL import Image

from PIL import GifImagePlugin

 

imageObject = Image.open("./flag_2.gif")

print(imageObject.is_animated)

print(imageObject.n_frames)

 

# Display individual frames from the loaded animated GIF file

for frame in range(0,imageObject.n_frames):

    imageObject.seek(frame)
    imageObject.save(f"flag_2/{frame}.png")

```

Then merge these individual frames.

```python
from PIL import Image

x = 1

i = Image.open(f"flag_{x}/0.png")

pixels = i.load()
width, height = i.size

for n in range(1, 19+x):
	with Image.open(f"flag_{x}/{n}.png") as i2:
		pixels2 = i2.load()
		for x in range(width):
			for y in range(height):
				try:
					if pixels2[x, y] != (255, 255, 255):
						pixels[x, y] = 255
				except:
					print(n)
					print(pixels[x, y], pixels2[x, y])
					continue

i.save(f"flag_{x}/merged{x}.png")
```

![https://i.imgur.com/MoSuOAv.png](merged-gifs)

Now, use ROT47 on obtained text (ASCII printable characters) to get 2 sets of RSA ciphers.

```python
import string
pchars = string.printable.strip()
#print(pchars)

asciiorder = [chr(i) for i in range(128)]

chars = ""
for i in asciiorder:
    if i in pchars:
        chars += i

print(chars)

def rot(s, n):
    ret = ""
    for i in s:
        for x in range(len(chars)):
            if chars[x] == i:
                ret += chars[(x+n)%len(chars)]
    return ret

parts = [
    "?`labaf_haf",
    "6`leddbf",
    "4`lbgfaegf",
    "?al``hfd_c_b",
    "6aladf",
    "4aldgd`hhce"
]

for p in parts:
    print(rot(p, 47))
    exec(rot(p, 47))

p1 = 4817
q1 = 4831
p2 = 3
q2 = 39916801

t1 = (p1-1)*(q1-1)
t2 = (p2-1)*(q2-1)

d1 = pow(e1, -1, t1)
d2 = pow(e2, -1, t2)

m1 = pow(c1, d1, n1)
m2 = pow(c2, d2, n2)

from Crypto.Util.number import long_to_bytes

print(long_to_bytes(m1) + long_to_bytes(m2))

```

Flag : Yos{835e0f}
