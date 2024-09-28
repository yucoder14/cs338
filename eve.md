Diffie-Hellman
==============
Given: 

$$\begin{align} g = 7, & p = 97\newline B = 53, & A = 82\end{align}$$

```
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

I used the brute force method to find that the shared secret was 1. 
To find the shared secret $K$, we must find $a, b$ such that 

$$K =\quad B^a\mod p\quad =\quad A^b\mod p \quad=\quad g^{ab}\mod p$$
So, I wrote a python script that increments a or b until the above condition is satisfied.

Because I'm using the brute force method, my process has the potential to take decades or centuries had the integers involved were much larger.

RSA
===
