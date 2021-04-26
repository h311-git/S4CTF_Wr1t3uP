# [S4CTF 2021](https://s4ctf.peykar.io):Baby-IQ

**Category:** Cryptography

**Description:** Use this simple crypto algorithm to learn about padding and evaluate yourself on how well you understand algorithms.

the challenge code is:
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from math import sqrt
from flag import flag
import os
import random
import base64

def chunkchunk(msg, l):
	return [msg[l*i:l*(i + 1)] for i in range(0, len(msg) // l)]

def pad(msg):
	r = int(sqrt(len(msg))) + 1
	head = base64.b64encode(os.urandom(r**2))[:r**2 - (len(msg))]
	msg = head + msg.encode('utf-8')
	msg = chunkchunk(msg, r)
	return [list(m) for m in msg]

def encrypt(A):
	row = len(A)
	col = len(A[0])
	top = 0
	left = 0
	tmp = []
	while (top < row and left < col) :       
		for i in range(left, col) : 
			tmp.append(A[top][i])              
		top += 1
		for i in range(top, row) : 
			tmp.append(A[i][col - 1])     
		col -= 1
		if ( top < row) : 
			for i in range(col - 1, left - 1, -1) : 
				tmp.append(A[row - 1][i])  
			row -= 1
		  
		if (left < col) : 
			for i in range(row - 1, top - 1, -1) : 
				tmp.append(A[i][left])   
			left += 1
	result = []
	for i in range(len(A)):
		r = []
		for j in range(len(A[0])):
			r.append(tmp[i*len(A[0]) + j])
		result.append(r)
	return result

A = pad(flag)
for _ in range(len(A)):
	_ = encrypt(A)
	A = _

print('enc =', A)

```
and we know:

```text
enc = [[122, 83, 52, 67, 84, 70], [89, 114, 79, 48, 67, 125], [95, 121, 114, 53, 116, 55], [123, 95, 80, 51, 52, 95], [102, 115, 114, 95, 119, 107], [52, 117, 109, 33, 97, 112]]

```
## solution overview

Ok we have this long python code which produces a list of lists which each list contains some ascii codes. let's just convert them to string.
the result is:
```text
zS4CTFYrO0C}_yr5t7{_P34_fsr_wk4um!ap
```
At the first sight Is it obvious that the flag characters is shuffled.
OK let's use a dummy flag `S4CTF{some_l33t_string_l1k3_7hi5}` to see what the pad function is doing. the result is:
```python
[[116, 118, 70, 83, 52, 67], [84, 70, 123, 115, 111, 109], [101, 95, 108, 51, 51, 116], [95, 115, 116, 114, 105, 110], [103, 95, 108, 49, 107, 51], [95, 55, 104, 105, 53, 125]]
```
And if we convert this ascii codes to string we have:
```
tvFS4CTF{some_l33t_string_l1k3_7hi5}
```
It seems the pad function takes the flag and adds some random string to the first of it and finally convert it to a list of lists which each list contains six ascii codes.
let's perfom the encrypt function on that to see what happens(i mean that for loop):

the result is:
```python
[[116, 118, 70, 83, 52, 67], [111, 95, 116, 95, 101, 125], [109, 108, 95, 114, 51, 115], [84, 110, 51, 70, 51, 95], [103, 115, 108, 49, 107, 105], [123, 104, 55, 53, 116, 105]]

```
and:

```text
tvFS4Co_t_e}ml_r3sTn3F3_gsl1ki{h75ti
```

we can see the encrypt function just shuffles the flag. looks like we have to read all the code to know what this function is doing but wait! if you think about it the encrypt function is 
just shuffling the given list. so it might be possible that this process goes around a loop this means after some round we could have the flag so let's try it:

```python
enc = [[122, 83, 52, 67, 84, 70], [89, 114, 79, 48, 67, 125], [95, 121, 114, 53, 116, 55], [123, 95, 80, 51, 52, 95], [102, 115, 114, 95, 119, 107], [52, 117, 109, 33, 97, 112]]


for i in range(1000):
	for _ in range(len(enc)):
		_ = encrypt(enc)
		enc = _
```

and the result is:


```text
0
zS4CTFC47s_p}ry3t_Y_5rk{f0r_w4OmuaP!

1
zS4CTF_O_0}!pyrrt_C{344Yfsr_wk7umP5a

2
zS4CTF}7_spa!ry4t{_YrOkCf0r_w4_mu53P

3
zS4CTFp_{0!PayrOtY}C474_fsr_wk_um3r5
.
.
.
.

32
zS4CTF{34sY_CryPtO_7a5k_f0r_w4rmup!}
```

Yeah we guessed right! at `32th` round we have the flag!

The flag is:`S4CTF{34sY_CryPtO_7a5k_f0r_w4rmup!}`



