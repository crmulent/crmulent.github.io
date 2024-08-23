---
title: picoCTF (Cryptography) - Writeup
time: 2024-07-16 12:00:00
categories: [cryptography]
tags: []
image: /assets/posts/picoctf/icon.svg
---

A writeup for picoGym's cryptography challenges.

# Crypto

## The Numbers
Flag: `picoCTF{thenumbersmason}`

The numbers... what do they mean?

Input
```
16 9 3 15 3 20 6 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14
```

CyberChef's Recipe
```
A1Z26_Cipher_Decode('Space')
```

Output
```
picoctfthenumbersmason
```

## 13
Flag: `picoCTF{not_too_bad_of_a_problem}`

Cryptography can be easy, do you know what ROT13 is? cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}

CyberChef's Recipe
```
ROT13(true,true,false,13)
```

## Mod 26
Flag: `picoCTF{next_time_I'll_try_2_rounds_of_rot13_TLcKBUdK}`

Cryptography can be easy, do you know what ROT13 is? cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_GYpXOHqX}

CyberChef's Recipe
```
ROT13(true,true,false,13)
```

## interencdec
Flag: `picoCTF{caesar_d3cr9pt3d_86de32d2}`

Can you get the real meaning from this file.

```
$ cat enc_flag
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6ZzJhMnd6TW1zeWZRPT0nCg==
```

```
From_Base64('A-Za-z0-9+/=',true,false)
Find_/_Replace({'option':'Regex','string':'b\''},'',true,false,true,false)
Find_/_Replace({'option':'Regex','string':'\''},'',true,false,true,false)
From_Base64('A-Za-z0-9+/=',true,false)
ROT13(true,true,false,19)
```

## rotation
Flag: `picoCTF{r0tat1on_d3crypt3d_4c71f5b0}`

You will find the flag after decrypting this file

Bruteforce rotation using CyberChef, key=18.


## Vigenere
Flag: `picoCTF{D0NT_US3_V1G3N3R3_C1PH3R_ae82272q}`

Can you decrypt this message?

Use [dcode](https://www.dcode.fr/vigenere-cipher) to decode the message. By knowing picoCTF as the flag format, we can acquire the key by bruteforcing.

## substitution0
Flag: `picoCTF{5UB5717U710N_3V0LU710N_03055505}`

A message has come in but it seems to be all scrambled. Luckily it seems to have the key at the beginning. Can you crack this substitution cipher?

Use [this tool](https://www.guballa.de/substitution-solver) to automatically break the substitution cipher.

## substitution1
Flag: `picoCTF{FR3QU3NCY_4774CK5_4R3_C001_4871E6FB}`

A second message has come in the mail, and it seems almost identical to the first one. Maybe the same thing will work again.

Use CyberChef to manually break the substitution cipher.

## substitution2
Flag: `picoCTF{N6R4M_4N41Y515_15_73D10U5_8E1BF808}`

It seems that another encrypted message has been intercepted. The encryptor seems to have learned their lesson though and now there isn't any punctuation! Can you still crack the cipher?

Use [this tool](https://www.guballa.de/substitution-solver) to automatically break the substitution cipher.

## caesar
Flag: `picoCTF{crossingtherubiconzaqjsscr}`

Decrypt this message.

Use CyberChef to bruteforce the cipher. It turns out the flag is encrypted using key 25.

## morse-code
Flag: `picoCTF{WH47_H47H_90D_W20U9H7}`

Morse code is well known. Can you decrypt this? Download the file here. Wrap your answer with picoCTF{}, put underscores in place of pauses, and use all lowercase.

Use [this tool](https://morsecode.world/international/decoder/audio-decoder-adaptive.html) to automatically decode the morse code in the wav file.
