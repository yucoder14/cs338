Sunday, 2024-10-06
author: Changwoo Yu

Scenario 1
==========

# Problem

Alice wants to send Bob a long message, and she doesn't want Eve to be able to read it. Assume for this scenario that AITM is impossible.

# Solution

Since AITM is impossible, we can just use the Diffie-Hellman Key exchange to first share a common key K. This way, Eve cannot plausibly calculate K. 

Then, Alice can use encryption algorithm AES to encrypt the message before sending it over to Bob.
```
C = AES(K, M)
```

When Bob receives the encrypted message C, he can decrypt the message using AES 
```
M = AES(K, C)
```

Scenario 2
==========

# Problem

Alice wants to send Bob a long message. She doesn't want Mal to be able to modify the message without Bob detecting the change.

# Solution

Upon Diffie-Hellman key exchange, it is possible the Mal conducted two-way key exchange, resulting in keys $K_1$ shared between Alice and Mal and $K_2$ shared between Bob and Mal. Assuming that Mal's primary objective to to modify Alice's message before transferring it to Bob, Mal will not drop any packets between Alice and Bob and will always attempt to modify them.

To specify the sources of data, we must proceed with public/secret key pairs.  

 


