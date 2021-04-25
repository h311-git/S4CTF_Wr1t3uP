# [S4CTF 2021](https://s4ctf.peykar.io):Baby-XoR

**Category:** Cryptography

**Description:** The first technique one should learn, in order to enter the fun world of modern cryptography, is XoR. Have you learned it well?

The challenge code is:
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from flag import flag

def xor(u, v):    
	return ''.join(chr(ord(cu) ^ ord(cv)) for cu, cv in zip(u, v))

u = flag
v = flag[1:] + flag[0]

enc = open('flag.enc', 'w')
enc.write(xor(u, v))
enc.close()
```

and we have a binary file containing some binary data which is produced from code above

## solution overview



