# [Crypto] ROT128

## 1. Challenge overview
- Category: Cryptography
- Difficulty: C1
- Objective: 1) analyze the encryption algorithm of the provided 'encfile'
             2) implement a decryption script using reverse operations
             3) restore the original flag image ('flag.png')

---

 ## 2. Logic analysis
After analyzing the encryption mechanism -> each byte of original data was procesed as follows:
1) original byte value (0-255) is converted into a 2-digit, uppercase hexadecimal string
2) specific arithmetic shift (+128) is applied to obfuscate the value

To recover original data, perform the exact inverse operation: '(encrypted_value - 128) % 256'.

---

## 3. Decrypt script
This is the Python script I wrote to reverse the encryption logic. It reads 'encfile', performs the modular inverse subtraction, and outputs the recovered bytes into an image.

```python
# Generate a static hex lookup table (00 ~ FF)
hex_list = [(hex(i)[2:].zfill(2).upper()) for i in range(256)]

# Read the encrypted file
with open ('encfile', 'r', encoding='utf-8') as f:
    enc_s = f.read()

enc_list = []
# Split the hex sting into 2-character chunks (byte units)
for i in range(0, len(enc_s), 2):
    enc_list.append(enc_s[i:i+2])

final_list = []
# Reverse the encryption operation to restore the original bytes
for hex_word in enc_list:
    hex_num = hex_list.index(hex_word)

    # Inverse opeation
    original_index = (hex_num - 128) % 256
    final_list.append(original_index)

# Convert to bytes and reconstruct the original flag.png
plain_bytes = bytes(final_list)
with open('flag.png', 'wb') as f:
    f.write(plain_bytes)

