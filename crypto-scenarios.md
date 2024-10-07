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

To specify the sources of data, we must proceed with public/secret key pairs. However, to ensure that Bob is able to check for any modification to the data, we must also use cryptographic hashing algorithm SHA-256. 

Before sending her long message, Alice first calculates the digest of M:
```
D = H(M)
``` 
Then, Alice encrypts the digest using her private key, creating $Sig_A$: 
```
Sig_A = E(S_A, D)
```
Unaware of AITM, Alice uses $K_1$ and AES to encrypt her message: 
```
C = AES(M, K_1)
```
Done with her calculations, Alice sends over $C$ and $Sig_A$ to Mal.

Upon receiving $C$ and $Sig_A$ from Alice, Mal has the capacity to decrypt $C$, for Alice and Mal shares the secrets:
```
M = AES(C, K_1)
```
Mal will than re-encrypt the message using $K_2$:
```
C_M = AES(M, K_2)
```
Mal can also decrypt $Sig_A$, for Mal knows Alice's public key. However, Mal cannot re-encrypt the data using Alice's private key. Because we are assuming that everyone has a correct copy of everybody else's key, if Mal were to encrypt $Sig_M$ using her own private key, Bob will know that the signature has been tempered by Mal. Thus, Mal has no choice but to just forward the $Sig_A$ to Bob, or Bob will discover that he has been talking to Mal.  

When Bob receives $C_M$ and $Sig_A$ from Mal, he can decrypt $C_M$ using $K_2$:
```
X = AES(C_M, K_2)
```
To check if $X$ has been tempered with, Bob can decrypt $Sig_A$ using $P_A$ and compare it with $H(X)$:
```
Y = E(P_A, Y)
```
If $Y = H(X)$, then Bob can be sure that the message has not been modified.

Scenario 3
==========

# Problem

Alice wants to send Bob a long message (in this case, it's a signed contract between AliceCom and BobCom), she doesn't want Eve to be able to read it, and she wants Bob to have confidence that it was Alice who sent the message. Assume for this scenario that AITM is impossible.

# Solution 

First, we proceed with Diffie-Hellman key exchange to get a shared secret K. Because it is impractical to use public/private key pair for long messages, Alice encrypts her message using K: 
```
C = AES(M, K)
```
Then, Alice creates a digest $D_A$ using SHA-256 hash function:
```
D_A = H(M) 
```
Then, Alice encrypts the digest using her private key $S_A$:
```
Sig_A = E(S_A, D_A) 
```
Finally, Alice encrypts it again with Bob's public key $P_B$:
```
X = E(P_B, Sig_A)
```
When Bob receives $C$ and $X$, he first decrypts $X$ with his secret $S_B$:
```
Sig_A = E(S_B, X)
```
Because $Sig_A$ can only be decrypted using Alice's public key, Bob can be confident that $Sig_A$ was from A:
```
Y = E(P_A, Sig_A)
```
After Bob decrypts $C$, he can then compare it with $Y$:
```
M_A = AES(C, K) 
```
If $H(M_A) == Y$, then Bob can be sure that the message was sent by Alice.

Scenario 4
==========

# Problem

Consider a scenario where Alice and Bob have been in contract negotiations and sharing documents electronically along the way. Suppose Bob sues Alice for breach of contract and presents as evidence the digitally signed contract (C || Sig) and Alice's public key P_A. Here, C contains some indication that Alice has agreed to the contract—e.g., if C is a PDF file containing an image of Alice's handwritten signature. Sig, on the other hand is a digital signature, as described at 9:23 or so of the Cryptographic Hash Functions video.

Suppose Alice says in court "C is not the contract I sent to Bob". (This is known as repudiation in cryptographic vocabulary.) Alice will now need to explain to the court what she believes happened that enabled Bob to end up with an erroneous contract. List at least three things Alice could claim happened. For each of Alice's claims, state briefly how plausible you would find the claim if you were the judge. (Assume that you, the judge, studied cryptography in college.)

# Solution 

## Case 1

Alice could claim that AITM occurred. If the decrypted digest from Alice's signature matches the hash of C, then Alice's claim loses credibility. However, if the decrypted digest disagrees with the hash of C, then there's evidence for Alice's claim. 

## Case 2

Alice could claim that AITM occurred and that the hash function experience a collision. In the case that the decrypted digest matches with the hash of C, Alice could claim that her digital signature has been forged. While not impossible, considering how a single bit of change can result in totally different hash, Alice's claim is implausible.

## Case 3

Alice could claim that Bob forged the contract. Had Bob somehow gotten hold of Alice's secret, then forging the document and changing the signature is in the realms of possibility. However, under the assumption that Alice have secured her secret, Alice's claim is not plausible.

## Case 4 

Alice could claim that P_A is not her public key. It is possible that Mal could have tricked Bob to think that Mal's public key was Alice's public key. However, under this assumption that everybody has a correct copy of everybody else's public key, her claim is implausible. 

Scenario 5
==========

# Problem

For this scenario, suppose the assumption that everybody has everybody else's correct public keys is no longer true. Instead, suppose we now have a certificate authority CA, and that everybody has the correct P_CA (i.e. the certificate authority's key). Suppose further that Bob sent his public key P_B to CA, and that CA then delivered to Bob this certificate:
```
Cert_B = "bob.com" || P_B || Sig_CA 
```
In terms of P_CA, S_CA, H, E, etc., of what would Sig_CA consist? That is, show the formula CA would use to compute Sig_CA.

# Solution

```
Sig_CA = E(S_CA, H(P_B)) 
```

Scenario 6
==========

# Problem

Bob now has the certificate Cert_B from the previous question. During a communication, Bob sends Alice Cert_B. Is that enough for Alice to believe she's talking to Bob? (Hint: no.) What could Alice and Bob do to convince Alice that Bob has the S_B that goes with the P_B in Cert_B?

# Solution

To ensure that Cert_B has the S_B that goes with the P_B in Cert_B, Bob should encrypt Sig_CA with S_B. 

In order to check if P_B goes along with S_B, Alice can try to decrypt E(S_B, Sig_CA) with P_B. Then, Alice can further check if purported P_B is the right public key by comparing H(P_B) with E(P_CA, Sig_CA). If the two are equal, then Alice can be confident that Bob has the S_B that goes with P_B in Cert_B. 

Scenario 7
==========

# Problem

Finally, list at least two ways the certificate-based trust system from the previous two questions could be subverted, allowing Mal to convince Alice that Mal is Bob.

# Solution

Mal can fool the Alice by conducting a AIMT between Alice and CA. If Mal can intercept Alice's request for Cert_B, Mal could return Cert_M instead.

Mal can fool the CA by conducting a AIMT between Bob and CA. If CA thinks Mal is Bob, then Cert_B will contain information regarding Mal's public key P_M, not P_B. 

If Mal gets a hold of S_CA, then Mal can issue incorrect certificates to fool Alice. 
