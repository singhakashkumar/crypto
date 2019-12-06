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
