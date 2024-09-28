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

Now find $d$, such that  $d * e \equiv 1 \mod (p - 1)(q-1)$
