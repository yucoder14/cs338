Ⅳ. Don't Write You Own Cryptography
===================================

### a. Textbook RSA
-------------------

Textbook RSA is deterministic encryption algorithm, i.e., it will always produce the same encryption given the same plain text messages. Because of its deterministic characteristics, attackers can conduct "chosen plain text attack" against RSA. In a chosen plain text attack, the attackers try to guess what the cipher text is by encrypting plain texts using the appropriate public key and then comparing it with the cipher text. If the encrypted plain text matches with the cipher text, the attacker just figured out the encrypted message.  

Another problem with textbook RSA is that it is vulnerable to chosen-cipher attack. In a chosen-cipher attack, the attacker exploits the fact that $m_1^em_2^e \equiv (m_1m_2)^e \mod n$: 
> an attacker who wants to know the decryption of a ciphertext c ≡ me (mod n) may ask the holder of the private key d to decrypt an unsuspicious-looking ciphertext c′ ≡ cre (mod n) for some value r chosen by the attacker. Because of the multiplicative property, c' is the encryption of mr (mod n). Hence, if the attacker is successful with the attack, they will learn mr (mod n), from which they can derive the message m by multiplying mr with the modular inverse of r modulo n

Source: https://en.wikipedia.org/wiki/RSA_(cryptosystem)

### b. Digital Signature Algorithm
----------------------------------

### C. Openssl Keygen
---------------------

Ⅴ. Hacking Jeff's Server One More Time
======================================

### a. First Secret

First use gobuster to find if any common named folder exists in the domain.

```
gobuster dir -u https://intrigue.jeffondich.com/ -w /usr/share/wordlists/dirb/common.txt
```

Gobuster found three things:
```
/admin
/index.php
/misc
```

/misc looked suspicious, so I navigated to https://intrigue.jeffondich.com/misc.

Then I found the "plans-and-secrets.txt" file:

```
Plans
- Finish the term
- Sleep for three days
- Eat cookies (the analog kind, not digital)

Secrets
- This is not really a secret
- The secret for the final exam is: (ANALOG) COOKIES ARE DELICIOUS
```

### b. Second Secret

To see what ports are open at the server that hosts intrigue.jeffondich.com, I used nmap to do port scanning.
```
nmap -sV intrigue.jeffondich.com
```

Nmap found that there are total of 5 ports open:
```
PORT      STATE    SERVICE  VERSION
22/tcp    open     ssh      OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
80/tcp    open     http     nginx 1.18.0 (Ubuntu)
443/tcp   open     ssl/http nginx 1.18.0 (Ubuntu)
646/tcp   filtered ldp
12345/tcp open     http     Python http.server 3.5 - 3.10
```

Port 12345 seemed suspicious, so I curl-ed:
```
curl intrigue.jeffondich.com:12345
```
I got:
```
Hi from Jeff's dog server! To get the secret, include Jeff's
dog's name as the value of the HTTP header "JO-Dog".
```

The name of Jeff's dog can be found in intrigue.jeffondich.com/jo-images/pets/. By clicking through each picture, it's not difficult to observe that maisie is a dog.

To send a HTTP GET request, I first initiated a tcp connection with nc:
```
nc intrigue.jeffondich.com 12345
```

Then, I send an HTTP GET request with Jeff's dog's name as the value of the HTTP header "JO-Dog":
```
GET / HTTP/1.0
JO-Dog: maisie
```

I get a HTTP response containing the second secret:
```
HTTP/1.0 200 OK
Server: BaseHTTP/0.6 Python/3.10.12
Date: Sat, 16 Nov 2024 22:21:33 GMT


Jeff's dog server says the secret is:

    The previous dog, Ruby, was a master of escape.
    At 105 pounds running full-tilt, she was impossible
    to tackle.

```

### c. Third Secret



Ⅵ. Recommendations
==================
