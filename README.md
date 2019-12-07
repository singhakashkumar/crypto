# CRYPTOGRAPHY

## Hex Encoding
Encoding strings to Hex can be done using python 2

```python
print "your_string".encode('hex')
```

**Nibble** corresponds to single hexadecimal digit.

Decoding Hex back to string 
```python
print "hex_String".decode('hex')
```
## Base64 Encoding

Base64 has 64 characters.

It takes **3 Bytes**

The following are the character set for Base64:
1. [A-Z] : 26 characters.
2. [a-z] : 26 characters.
3. [0-9] : 10 characters.
4. [+] : 1 character.
5. [/] : 1 character.

It also has '=' character which is solely used for padding purpose.

*Encoding process is simple. Write down the binary of the message. Take groups of 6 in one block. Now compare each block with the decimal value of the corresponding character in the Base64 Chart. Join the characters, and that's it.*

***Tip to identify base64:*** if an encoded string contains '=' at the end then it is base64 encoded we need to decode it using base64.
```python
print "your_string".encode('base64') # encoding string to base64
print "encoded_string".decode('base64') # decode into normal string
```
## Hashing
***Hash Function:*** It takes a message of any size as plaintext and gives out a fixed-sized string as ciphertext. The ciphertext, is an alphanumeric string (containing both letters and numerals) and is called a DIGEST.

In this no key is used. It is one way function. Once a message is hashed then it is impossible to get the message back.

Every hash output is unique for different inputs and same for same inputs. Hashing is not an Encryption. It is a fingerprint of a message.

**Tool:** Crack Station.
## Cipher
ROT13(ROT13(X)) == X (Substitution Cipher).
### XOR Encryption
XOR of two number is done using xor operator(^)
```python
>>> 5^9
12
```
XOR of two character 
when you xor two characters, these are first converted into ascii form and then xored together.
ord() function is used to convert char to its respective ascii.
```python
>>> ord('r')
114
>>> ord('j')
106
>>> chr(ord('r') ^ ord('j'))
\x18
```
#### Single Byte XOR cipher

```
                   H         e         l         l         o
                01001000  01100101  01101100  01101100  01101111    
              ⊕ 01110011  01110011  01110011  01110011  01110011
                   s         s         s         s         s
                ------------------------------------------------
                00111011  00010110  00011111  00011111  00011100
                   ;        \x16      \x1f      \x1f      \x1c
```
```
                   ;        \x16      \x1f      \x1f      \x1c
                00111011  00010110  00011111  00011111  00011100
              ⊕ 01110011  01110011  01110011  01110011  01110011
                   s          s         s         s         s
                ------------------------------------------------
                01001000  01100101  01101100  01101100  01101111    
                    H         e         l         l         o
```
#### Repeated Key XOR Cipher
**Message:** Document
**Key:** abc
```
      D         o         c         u         m        e         n         t
  01000100  01101111  01100011  01110101  01101101  01100101  01101110  01110100    
⊕ 01100001  01100010  01100011  01100001  01100010  01100011  01100001  01100010
      a         b         c         a         b        c         a         b
  ------------------------------------------------------------------------------
  00100101  00001101  00000000  00010100  00001111  00000110  00001111  00010110
      %        \r       \x00      \x14      \x0f      \x06      \x0f      \x16
```
```
      %        \r       \x00      \x14      \x0f      \x06      \x0f      \x16
  00100101  00001101  00000000  00010100  00001111  00000110  00001111  00010110
⊕ 01100001  01100010  01100011  01100001  01100010  01100011  01100001  01100010
      a         b         c         a         b        c         a         b
  ------------------------------------------------------------------------------
  01000100  01101111  01100011  01110101  01101101  01100101  01101110  01110100    
      D         o         c         u         m        e         n         t
```
**Python Code:**
```python
>>> pt = "Document"
>>> key = "abcabcab"
>>> "".join(chr(ord(i) ^ ord(j)) for i,j in zip(pt,key))
'%\r\x00\x14\x0f\x06\x0f\x16'
>>> ct = '%\r\x00\x14\x0f\x06\x0f\x16'
>>> "".join(chr(ord(i) ^ ord(j)) for i,j in zip(ct,key))
'Document'
```

## RSA - Public Key Crypto System.

It is based on the principle of factorizing large prime numbers.

Now let us see the several steps used for encryption/decryption:

1. Key Generation
2. Key Distribution
3. Encryption/Decryption

Variables which are Generally used are:
1. **p, q**: two large prime numbers
2. **n**: modulus, n = p * q
3. **e**: public key exponent
4. **d**: private key exponent
5. **M**: unpadded message
6. **m**: padded message
7. **c**: cipher text
8. **φ(n)**: Euler's totient function

### Key Generation

1. Choose two distinct primes p and q. Both the primes must be chosen randomly and must be similar in magnitude. There should be a significant difference in the values of the primes otherwise it will become vulnerable to Fermat's Factorisation and we will see how and why is it so.
2. Compute n=p∗q
3. Calculate the value of φ(n)=(p−1)∗(q−1) in this case. 
4. Choose an integer e such that 1<e<φ(n) and gcd(e,φ(n))=1. e is often selected small and to be in the form of 22i+1 (where i is a positive integer) also known as Fermat Numbers, because it makes exponentiation more efficient. This makes it vulnerable to some attacks in some situations and we will see how we can prevent them.
5. Compute d as d=e<sup>−1</sup>modφ(n)
6. The pair (e,n) is known as public key and (d,n) is known as private key.

### Encryption

1. Convert the message into integer form
2. Computes ciphertext c=m<sup>e</sup>mod n
3. Now you have the ciphertext c ready to transmit using a reliable channel

### Decryption

1. Computes m=c<sup>d</sup>mod n . How this provides us the message we will see in the next section.
2. Convert the integer plaintext to the normal form.

### Implementation

```python
from Crypto.PublicKey import RSA
from Crypto.Util.number import *
f=open('PublicKey.pem').read()
n=RSA.importKey(f).n
e=RSA.importKey(f).e
n = p*q
phin = (p-1)*(q-1)
d = inverse(e,phin)
plaintext = pow(ciphertext,d,n)
```

```python
>>> bytes_to_long('RSA is cool')
99525075000367028712206188L
>>> long_to_bytes(99525075000367028712206188L)
'RSA is cool'
```
