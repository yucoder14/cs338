Diffie-Hellman
==============
Given: 

$$\begin{align} g = 7, & p = 97\newline B = 53, & A = 82\end{align}$$

## Brute force 

```
# initial conditions
g = 7
p = 97
A = 82
B = 53

BMod = B
AMod = A

a = 1
b = 1

while (BMod != AMod) or (g**(a*b)%p != BMod): 
    if BMod > AMod: 
        a +=1
        BMod = BMod*B%p
    else: 
        b += 1
        AMod = AMod*A%p

print(a: ", a, "b: ", b)
print("AMod: ", A**b%p) 
print("BMod: ", B**a%p) 
print("The key is: ", g**(a*b)%p)
```
Results from above script: 
```
a:  48 b:  96
AMod:  1
BMod:  1
The key is:  1
```

## Reflection

I used the brute force method to find that the shared secret was 1. 
To find the shared secret $K$, we must find $a, b$ such that 

$$K =\quad B^a\mod p\quad =\quad A^b\mod p \quad=\quad g^{ab}\mod p$$

So, I wrote a python script that increments $a$ or $b$ until the above condition is satisfied.

Because I'm using the brute force method, my process has the potential to take decades or centuries had the integers involved were much larger. 

RSA
===
Given: 
```
(e_Bob, n_Bob) = (13, 162991)
```
Encrypted Data:
```
[17645, 100861, 96754, 160977, 120780, 90338, 130962, 74096,
128123, 25052, 119569, 39404, 6697, 82550, 126667, 151824,
80067, 75272, 72641, 43884, 5579, 29857, 33449, 46274,
59283, 109287, 22623, 84902, 6161, 109039, 75094, 56614,
13649, 120780, 133707, 66992, 128221]
```

n = 162991
e = 13 

## Find the prime factors

Given $n$, we must first find prime numbers $p$ and $q$ such that $n = p \cdot q$.
Using Fermat's Factorization method
```
import math
def fermat_factor(n):
    a = math.ceil(math.sqrt(n))
    b = a*a - n
    while math.ceil(math.sqrt(b))**2 != b: 
        a += 1
        b = a*a - n

    return a - b**0.5 

n = 162991
p = fermat_factor(n)
q = n // p 
print("p: ", p, "q: ", q)
```

result:
```
p:  389.0 q:  419.0
```

## Finding $\lambda(n)

Find $\lambda(n) = \lcm(p - 1, q - 1)$: 
```
def gcd(a,b): 
    if a == 0: 
        return b

    return gcd(b % a, a)

def lcm(a, b): 
    return a * b / gcd (a,b)

ps = 389 - 1
qs = 419 - 1 

print(lcm(ps,qs))

```
result:
```
81092.0
```

## Finding the Secret Key

```

m = 81092
e = 13

d = 1

while e * d % m != 1 :
    d += 1

print(d)
```

result:
```
43665
```

So we have that our secret S is 
```
(d_Bob, n_Bob) = (43665, 162991)
```

## Decrypting message

Now decrypt the message:
```
d = 43665
n = 162991
print([(c**d%n) for c in [17645, 100861, 96754, 160977, 120780, 90338, 130962, 74096,
128123, 25052, 119569, 39404, 6697, 82550, 126667, 151824,
80067, 75272, 72641, 43884, 5579, 29857, 33449, 46274,
59283, 109287, 22623, 84902, 6161, 109039, 75094, 56614,
13649, 120780, 133707, 66992, 128221]])
```
result: 
```
[17509, 24946, 8258, 28514, 11296, 25448, 25955, 27424, 29800, 26995, 8303, 30068, 11808, 26740, 29808, 29498, 12079, 30583, 30510, 29557, 29302, 25961, 27756, 24942, 25445, 30561, 29795, 26670, 26991, 12064, 21349, 25888, 31073, 11296, 16748, 26979, 25902]
```

The integers are unrecognizable in decimal form. How about hex codes? 
```
d = [17509, 24946, 8258, 28514, 11296, 25448, 25955, 27424, 29800, 26995, 8303, 30068, 11808, 26740, 29808, 29498, 12079, 30583, 30510, 29557, 29302, 25961, 27756, 24942, 25445, 30561, 29795, 26670, 26991, 12064, 21349, 25888, 31073, 11296, 16748, 26979, 25902]

hex_codes = []

for i in d: 
   hex_codes.append(hex(i)) 

print(hex_codes)
```
result:
```
['0x4465', '0x6172', '0x2042', '0x6f62', '0x2c20', '0x6368', '0x6563', '0x6b20', '0x7468', '0x6973', '0x206f', '0x7574', '0x2e20', '0x6874', '0x7470', '0x733a', '0x2f2f', '0x7777', '0x772e', '0x7375', '0x7276', '0x6569', '0x6c6c', '0x616e', '0x6365', '0x7761', '0x7463', '0x682e', '0x696f', '0x2f20', '0x5365', '0x6520', '0x7961', '0x2c20', '0x416c', '0x6963', '0x652e']
```
It appears that each block has size of 16 bits or 2 bytes. Perhaps each byte is a printable character...


## Decoding the message

Now we try decoding using base64: 
```
d = [17509, 24946, 8258, 28514, 11296, 25448, 25955, 27424, 29800, 26995, 8303, 30068, 11808, 26740, 29808, 29498, 12079, 30583, 30510, 29557, 29302, 25961, 27756, 24942, 25445, 30561, 29795, 26670, 26991, 12064, 21349, 25888, 31073, 11296, 16748, 26979, 25902]

decoded = []

for i in d: 
    bit_mask = 0b11111111
    # first byte 
    decoded.append(chr(i >> 8))
    # second byte
    decoded.append(chr(i & bit_mask)) 

print("".join(decoded))

```
result: 
```
Dear Bob, check this out. https://www.surveillancewatch.io/ See ya, Alice.
```

## Reflection
