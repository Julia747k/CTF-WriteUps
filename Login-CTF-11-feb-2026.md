# LOGIN CTF 11.02.2026 
## Weird Message

<img width="472" height="322" alt="image" src="https://github.com/user-attachments/assets/c86328b8-1a07-4b4a-a297-6b94901593d7" />

Its most likely base64-decoded not base64-encoded.
Those hex bytes, when base64-encoded turn into: 

CTFkom{dec0d1ng_in5te4d_0f_enc0d1ng}


## base4? 

<img width="490" height="296" alt="image" src="https://github.com/user-attachments/assets/7b9664ab-9bd7-486d-82fa-6cffec0d6fae" />

The challenge title (“base4?”) and the fact that the weird message only contained four distinct letters — s, e, a, l — immediately suggested some kind of base-4 encoding.
Since there are exactly four unique characters, I mapped them to base-4 digits. 
The order “seal” which is a strong hint for the digit ordering. So I assigned
s = 0
e = 1
a = 2
l = 3

Next, because base-4 digits can represent binary data I grouped the encoded string into chunks of 4 characters. 
Four base-4 digits represent values from 0–255 (since 4⁴ = 256), which matches the range of one ASCII byte.
For each 4-character block:

1. I convert the letters to base-4 digits.
2. Interpret that as a base-4 number.
3. Convert it to decimal.
   
Translate the decimal value to its ASCII character.
Doing this for the entire string give us the flag: 




