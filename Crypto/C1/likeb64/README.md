# [Crypto] Custom Base32

## 1. Challenge overview
- Category: Cryptography
- Difficulty: C1
- Objective: 1) analyze the custom encoding mechanism of the provided 'target.txt'
             2) implement a decryption script using reverse alphabet mapping
             3) restore the original flag string

---

## 2. Logic analysis
After analyzing the encryption mechanism -> the challenge creator obfuscated the data as follows:
1) original data was encoded using a modified Base32 algorithm
2) specific custom alphabet (`ABCDEFGHIJKLMNOPQRSTUVWXYZabcdef`) was used instead of the standard one

To recover original data, perform the exact inverse operation: map the custom 32-character alphabet back to the standard Base32 alphabet (`ABCDEFGHIJKLMNOPQRSTUVWXYZ234567`) and decode.

---

## 3. Decrypt script
This is the Python script I wrote to reverse the encryption logic. It reads 'target.txt', performs the character translation mapping, and outputs the recovered flag string.

```python
import base64

# Read the encrypted file
with open('target.txt', 'r', encoding='utf-8') as f:
    target = f.read()

cipher_text = target

# Define custom and standard alphabet strings for 1:1 mapping
custom_alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdef"
standard_alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ234567"

# Generate a static translation lookup table
translation_table = str.maketrans(custom_alphabet, standard_alphabet)

# Substitute the custom characters to restore the standard Base32 string
translated_cipher = cipher_text.translate(translation_table)

# Decode the standard Base32 string and reconstruct the original flag
decoded_bytes = base64.b32decode(translated_cipher)
flag = decoded_bytes.decode('utf-8')

# Print the final recovered flag
print(flag)
