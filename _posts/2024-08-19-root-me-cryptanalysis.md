---
title: Root-me (Cryptanalysis) - Writeup
time: 2024-08-19 12:00:00
categories: [cryptography]
tags: []
image: /assets/posts/root-me/icon.svg
---

A writeup for Root-me's cryptanalysis challenges.

## Encoding - ASCII
Flag: `2ac376481ae546cd689d5b91275d324e`

Decode the string.

Input
```
4C6520666C6167206465206365206368616C6C656E6765206573743A203261633337363438316165353436636436383964356239313237356433323465
```

CyberChef's Recipe
```
From_Hex('None')
```

Output
```
Le flag de ce challenge est: 2ac376481ae546cd689d5b91275d324e
```

## Encoding - UU
Flag: `ULTRASIMPLE`

Very used by the HTTP protocol

Get the validation password.

Input
```
B5F5R>2!S:6UP;&4@.RD*4$%34R`](%5,5%)!4TE-4$Q%"@``
```

[Output](https://www.browserling.com/tools/uudecode)
```
Very simple ;)
PASS = ULTRASIMPLE
```

## Hash - Message Digest 5
Flag: `weak`

Bob found that Ronald Rivest is a terrific cryptanalyst.

Crack the given hash.

Input
```
7ecc19e1a0be36ba2c6f05d06b5d3058
```

[Output](https://md5decrypt.net/en/)
```
7ecc19e1a0be36ba2c6f05d06b5d3058 : weak
```

## Hash - SHA-2
Flag: `a7c9d5a37201c08c5b7b156173bea5ec2063edf9`

This hash was stolen during a session interception on a critical application, errors may have occurred during transmission. No crack attempt has resulted so far; hash format seems unknown. Find the corresponding plaintext.

The answer is the SHA-1 of this password.



Hashes must have a-f and 0-9 characters only.
Following this rule we must exclude the "k" from the hash.

So,
96719db60d8e3f498c98d94155e1296aac105ck4923290c89eeeb3ba26d3eef92
becomes
96719db60d8e3f498c98d94155e1296aac105c4923290c89eeeb3ba26d3eef92

Input
```
96719db60d8e3f498c98d94155e1296aac105c4923290c89eeeb3ba26d3eef92
```

[Output](https://md5decrypt.net/en/Sha256/)
```
96719db60d8e3f498c98d94155e1296aac105c4923290c89eeeb3ba26d3eef92 : 4dM1n
```

Input
```
4dM1n
```

CyberChef's Recipe
```
SHA1(80)
```

[Output](https://gchq.github.io/CyberChef/)
```
a7c9d5a37201c08c5b7b156173bea5ec2063edf9
```

## Shift cipher
Flag: `Yolaihu`

Recover the password

Index : keep on turning.

Input
```
L|ky+*^*zo*kvsno|*kom*vo*zk}}*cyvksr
```

[Output](https://www.dcode.fr/unicode-shift-cipher)
* Shift = 10
```
Brao! Tu peu alider aec le pass Yolaihu
```

## Monoalphabetic substitution - Caesar
Flag: `ujqcsddessxsffes`

Emperor regresses

We just caught the messenger of the Emperor. He transmitted a coded message to his son. This could be an important message. You’ve to decrypt it ! To validate, you must enter the concatenation of the first letters of each line followed by the concatenation of the last letters of each line (for example : tfhqdlhfpkmeokgq).

Input
```
tm bcsv qolfp
f'dmvd xuhm exl tgak
hlrkiv sydg hxm
qiswzzwf qrf oqdueqe
dpae resd wndo
liva bu vgtokx sjzk
hmb rqch fqwbg
fmmft seront sntsdr pmsecq
```

The title hints that this challenge is a caesar cipher. After several trials, I found out that the plaintext is in French, making it harder to decode. We can decode the whole text by incrementing the shift of caesar by 1 for each word.

* key=1
```
un cdtw rpmgq
g'enwe yvin fym uhbl
<...>
```

* key=2
```
vo deux sqnhr
h'foxf zwjo gzn vicm
<...>
```

* key=3
```
wp efvy trois
i'gpyg axkp hao wjdn
<...>
```

Retaining each plaintext, we have the following:
```
un deux trois
```

which is **1, 2, 3** in French.

Plaintext
```
un deux trois
j'irai dans les bois
quatre cinq six
cueillir des cerises
sept huit neuf
dans un panier neuf
dix onze douze
elles seront toutes rouges
```

## Pixel Madness
Flag: `SOLUTION`

Decrypt the code.

Clue :
0 = #FFFFFF
1 = #000000

Submit password in CAPITAL LETTERS.

Use the following code to solve the challenge.
```python
from PIL import Image

data = [
            "0x3+1x1+0x1+0x1+0x7+1x2+0x15+1x1+0x8+1x1+0x8+1x1+0x1+1x1+0x1+1x1+0x1+1x1+0x1+1x1+0x3+1x1+0x1+1x1+0x3+1x1+0x1+1x4+0x2+1x1+0x25",
            "0x2+1x1+0x4+1x1+0x4+1x3+0x1+1x2+0x2+1x8+0x11+1x4+0x1+1x3+0x6+1x2+0x4+1x1+0x4+1x2+0x7+1x4+0x4+1x2+0x7+1x2+0x3+1x2+0x3",
            "0x3+1x1+0x2+1x1+0x2+1x1+0x11+1x2+0x2+1x3+0x7+1x1+0x4+1x2+0x2+1x2+0x7+1x1+0x6+1x1+0x2+1x1+0x4+1x3+0x1+1x1+0x4+1x1+0x2+1x1+0x2+1x1+0x3+1x1+0x2+1x3+0x2+1x2+0x3",
            "1x1+0x2+1x1+0x4+1x1+0x2+1x1+0x1+1x1+0x2+1x1+0x2+1x1+0x1+1x2+0x2+1x2+0x1+1x2+0x3+1x1+0x3+1x1+0x2+1x2+0x1+1x3+0x3+1x1+0x2+1x1+0x4+1x2+0x1+1x1+0x4+1x1+0x3+1x2+0x12+1x2+0x1+1x1+0x3+1x7+0x3",
            "0x3+1x1+0x7+1x1+0x1+1x1+0x4+1x1+0x2+1x2+0x2+1x2+0x4+1x1+0x2+1x1+0x1+1x2+0x1+1x8+0x1+1x1+0x4+1x1+0x5+1x1+0x3+1x2+0x2+1x1+0x1+1x2+0x2+1x1+0x3+1x2+0x9+1x1+0x1+1x2+0x2+1x3+0x2+1x1",
            "0x7+1x1+0x4+1x1+0x4+1x1+0x1+1x1+0x1+1x7+0x3+1x1+0x1+1x2+0x3+1x1+0x1+1x6+0x1+1x1+0x3+1x1+0x2+1x1+0x14+1x2+0x8+1x1+0x10+1x2+0x3+1x2+0x1+1x1+0x1",
            "0x6+1x5+0x4+1x1+0x7+1x1+0x2+1x1+0x3+1x2+0x4+1x1+0x8+1x1+0x3+1x2+0x1+1x2+0x3+1x1+0x8+1x1+0x2+1x2+0x1+1x1+0x3+1x7+0x5+1x2+0x2+1x1+0x2+1x2+0x3",
            "0x1+1x1+0x2+1x1+0x1+1x2+0x5+1x1+0x6+1x2+0x3+1x1+0x2+1x1+0x1+1x2+0x20+1x8+0x1+1x1+0x1+1x1+0x4+1x2+0x3+1x1+0x2+1x2+0x3+1x2+0x7+1x2+0x3+1x2+0x4",
            "0x2+1x1+0x3+1x5+0x5+1x2+0x7+1x1+0x4+1x2+0x2+1x1+0x2+1x2+0x1+1x1+0x3+1x1+0x6+1x2+0x2+1x2+0x3+1x2+0x2+1x3+0x1+1x1+0x6+1x3+0x3+1x5+0x3+1x1+0x4+1x1+0x5",
            "0x4+1x2+0x3+1x2+0x3+1x1+0x5+1x2+0x2+1x1+0x1+1x1+0x1+1x1+0x1+1x2+0x9+1x1+0x3+1x1+0x2+1x1+0x1+1x1+0x2+1x1+0x1+1x2+0x2+1x1+0x2+1x1+0x1+1x1+0x4+1x3+0x1+1x1+0x2+1x2+0x3+1x2+0x3+1x1+0x5+1x1+0x4+1x1+0x2",
            "0x6+1x5+0x4+1x1+0x1+1x1+0x2+1x2+0x6+1x1+0x1+1x7+0x4+1x3+0x3+1x1+0x4+1x1+0x2+1x2+0x4+1x1+0x6+1x1+0x6+1x8+0x3+1x1+0x5+1x1+0x7",
            "0x2+1x1+0x3+1x6+0x4+1x1+0x1+1x3+0x4+1x1+0x2+1x2+0x4+1x1+0x5+1x1+0x2+1x1+0x3+1x2+0x3+1x1+0x2+1x3+0x1+1x1+0x2+1x2+0x3+1x3+0x2+1x3+0x9+1x1+0x4+1x2+0x7+1x2",
]

# Function to know the max width
x = lambda x: [int(i[2]) for i in x.split('+')]
max_width = max([sum(i) for i in list(map(x, data))])
print(([sum(i) for i in list(map(x, data))]))

im = []

for row in data:
    k = 0
    for column in row.split('+'):
        column = list(map(int,column.split('x')))
        if column[0] == 0:
            for i in range(column[1]):
                if k == 100: break
                im.append((255,255,255))
                k+=1
        elif column[0] == 1:
            for i in range(column[1]):
                im.append((0,0,0))
                k+=1

image = Image.new("RGB", (max_width, len(data)))
image.putdata(im)
image.show()

# SOLUTION
```

## File - PKZIP
Flag: `14535`

A protected ZIP file, you have to find what’s inside.

Use zip2john and john to crack the password
```
$ zip2john ch5.zip > hash.txt
$ john hash.txt 
<...>
14535            (ch5.zip/readme.txt)     
<...>
```

## Polyalphabetic substitution - Vigenère
Flag: `Loyd Blankenship`

We need your expert opinion on this document. This is an old letter and it appears that it is important for the pirates that we are searching for. Your mission is to decipher the text and give us the full name of the author (example : "John Doe").

Input
```
Moi Tepdsi Fhrujrlhf

Nu egxex g'vla jmmg ifvgkvq ehcclkk'lgm, p'xgk ihvfshm rrgz pqw whiighyj. "Wptbutsi: Gr nwccxzgqrg tfixai bshk qibti urshfdtamcyr,
"Tfixzxmxvhb u'nu 'lmgxxf' riyie pr iwitaesi q'nbv uhrcyr"...

Loktuie kblgvl, asgw yxg dxtie.
<...>
```

[Output](https://www.dcode.fr/vigenere-cipher)
* Key = THEMENTOR
```
THEHACKERMANIFESTOUNAUTRESESTFAITPRENDREAUJOURDHUICESTPARTOUTDANSLESJOURNAUXSCANDALEUNADOLESCENTARRETEPOURCRIMEINFORMATIQUEARRESTATIONDUNHACKERAPRESLEPIRATAGEDUNEBANQUESATANESGOSSESTOUSLESMEMESMAISAVEZVOUSDANSVOTREPSYCHOLOGIEENTROISPIECEETVOTREPROFILTECHNOCRATIQUEDEUNJOURPENSEAREGARDERLEMONDEDERRIERELESYEUXDUNHACKERNEVOUSETESVOUSJAMAISDEMANDECEQUILAVAITFAITAGIRQUELLESFORCESLAVAIENTMODELEJESUISUNHACKERENTREZDANSMONMONDELEMIENESTUNMONDEQUICOMMENCEAVECLECOLEJESUISPLUSASTUCIEUXQUELAPLUPARTDESAUTRESENFANTSLESCONNERIESQUILSMAPPRENNENTMELASSENTJESUISAUCOLLEGEOUAULYCEEJAIECOUTELESPROFESSEURSEXPLIQUERPOURLAQUINZIEMEFOISCOMMENTREDUIREUNEFRACTIONJELAICOMPRISNONMMEDUBOISJENEPEUXPASMONTRERMONTRAVAILJELAIFAITDANSMATETESATANEGOSSESILLACERTAINEMENTCOPIETOUSLESMEMESJAIFAITUNEDECOUVERTEAUJOURDHUIJAITROUVEUNORDINATEURATTENDSUNEMINUTECESTCOOLCAFAITCEQUEJEVEUXSIOXDMLLCEYJOPQXJKVMYMYDFWYLYOBKQVMQJJQXLFHHIJJFOAQTMQCHJJYUPWXRMSFNMUUMHONIQQVWVKGJKYOHHIIGTFLUSSZTYVRGXSWVJYVRCVHKCZMUBRUWXQCIZULUSSZTYVRGXQSQDYUXQ
```

To make it more readable, let's decode it using CyberChef.

CyberChef's Recipe
```
Vigenère_Decode('THEMENTOR')
```

Output
```
The Hacker Manifesto

Un autre s'est fait prendre aujourd'hui, c'est partout dans les journaux. "Scandale: Un adolescent arrete pour crime informatique,
"Arrestation d'un 'hacker' apres le piratage d'une banque"...

Satanes gosses, tous les memes.
<...>
```

By searching the web about **The Hacker Manifesto Author**, I found out that the author was **Loyd Blankenship**.

## File - Insecure storage 1
Flag: `F1rstP4sSw0rD`

Mozilla Firefox 14

Retrieve the user’s password.

To retrieve the user's password, I used the tool [firepwd](https://github.com/lclevy/firepwd).

```
$ python3 firepwd.py -d ../.mozilla/firefox/o0s0xxhl.default/
 SEQUENCE {
   SEQUENCE {
     OBJECTIDENTIFIER 1.2.840.113549.1.12.5.1.3 pbeWithSha1AndTripleDES-CBC
     SEQUENCE {
       OCTETSTRING b'cdc0d5a7ce257959a27755b057ba3764afa6ecc6'
       INTEGER b'01'
     }
   }
   OCTETSTRING b'842aac4fc88d6c99993695d323dc4440576318024446f6fbf3131de5ca5cf524b2661f1efab6ce736ef94b67c8d18c0bbcd8741b4a363ad4adb8c228bb5e6e644fdfdcb599a8f6fd00f0d6fb45f347d43154ce129b917efc180205190539e4ca'
 }
decrypting privKeyData
 SEQUENCE {
   INTEGER b'00'
   SEQUENCE {
     OBJECTIDENTIFIER 1.2.840.113549.1.1.1 pkcs-1
     NULL 0
   }
   OCTETSTRING b'3043020100021100f8000000000000000000000000000001020100021900fecb1985f7f1522089086b0894e6a8ab684507321af2f11f020100020100020100020100020115'
 }
decoding b'3043020100021100f8000000000000000000000000000001020100021900fecb1985f7f1522089086b0894e6a8ab684507321af2f11f020100020100020100020100020115'
 SEQUENCE {
   INTEGER b'00'
   INTEGER b'00f8000000000000000000000000000001'
   INTEGER b'00'
   INTEGER b'00fecb1985f7f1522089086b0894e6a8ab684507321af2f11f'
   INTEGER b'00'
   INTEGER b'00'
   INTEGER b'00'
   INTEGER b'00'
   INTEGER b'15'
 }
sqlite
decrypting login/password pairs
http://www.root-me.org:b'shell1cracked',b'F1rstP4sSw0rD'
```

## Known plaintext - XOR
Flag: `ICONOCLASTE`

For this challenge you will need to decypher a simple XORed picture.

This BMP picture was mistakenly encrypted. Can you recover it ?

I used this [tool](https://wiremask.eu/tools/xor-cracker/) to know the possible keys used to encrypt the image. After getting the possible key, I used CyberChef to decrypt the image.

CyberChef's Recipe
```
XOR({'option':'UTF8','string':'fallen'},'Standard',false)
Render_Image('Raw')
```

## ELF64 - PID encryption
Flag: ``

Bad idea to use predictable stuff.

Source Code
```
/*
 * gcc ch21.c -lcrypt -o ch21
 */
 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <crypt.h>
#include <sys/types.h>
#include <unistd.h>
 
int main (int argc, char *argv[]) {
    char pid[16];
    char *args[] = { "/bin/bash", "-p", 0 };
 
    snprintf(pid, sizeof(pid), "%i", getpid());
    if (argc != 2)
        return 0;
 
    printf("%s=%s",argv[1], crypt(pid, "$1$awesome"));
 
    if (strcmp(argv[1], crypt(pid, "$1$awesome")) == 0) {
        printf("WIN!\n");
        execve(args[0], &args[0], NULL);
 
    } else {
        printf("Fail... :/\n");
    }
    return 0;
}
```

## Hash - LM
Flag: `admin!!`

Retrieve the password of the Administrator user from the information output by the secretsdump tool of the Impacket suite.

Note: the flag is in lowercase

Input
```
d3bf255c530633b9aad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0
```

Output
```
31d6cfe0d16ae931b73c59d7e0c089c0:
d3bf255c530633b9aad3b435b51404ee:ADMIN!!
```

## Hash - DCC
Flag: `ilikethat`

Retrieve the password of the Administrator user from the information output by the secretsdump tool of the Impacket suite.

Input
```
Administrator:15a57c279ebdfea574ad1ff91eb6ef0c
```

Output
```
$ john --format=mscash --wordlist=rockyou.txt hash.txt
<...>
ilikethat        (Administrator)     
<...>
```

## System - Android lock pattern
Flag: `145263780`

Having doubts about the loyalty of your wife, you’ve decided to read SMS, mail, etc in her smarpthone. Unfortunately it is locked by schema. In spite you still manage to retrieve system files.

You need to find this test scheme to unlock smartphone.

NB : validation password is a number (archive sha256 is 525daa911d4dddb7f3f4b4ec24bff594c4a1994b2e9558ee10329144a6657f98)

Resources

Procedure:
    https://www.forensicfocus.com/articles/android-forensics-study-of-password-and-pattern-lock-protection/

SHA-1 Dictionary:
    https://github.com/Machiry/AndroidGestureBreaker/blob/master/AndroidGestureSHA1.txt

Tool
    https://github.com/bolisettynihith/android-pattern-decoder

    https://github.com/MGF15/P-Decode

```
$ python3 android-pattern-decoder/androidpatterndecode.py -g ./android/data/system/gesture.key -d AndroidGestureSHA1.txt 
[+] Pattern retrieved from gesture.key file is: 256374891
```

```
$ python3 P-Decode/P-Decode.py -f ./android/data/system/gesture.key 

        |~)  |~\ _ _ _  _| _
        |~ ~~|_/}_(_(_)(_|}_ v0.5

             [ {41}ndr0id Pa77ern Cr4ck t00l. ]
       
[*] Pattern SHA1 Hash   : 2C3422D33FB9DD9CDE87657408E48F4E635713CB

[+] Pattern Length      : 9

[+] Pattern             : 145263780

[+] Pattern SVG         : 145263780.svg

[*] Time                : 0.4 sec
```

## Hash - DCC2
Flag: `ihatepasswords`

Retrieve the password of the Administrator user from the information output by the secretsdump tool of the Impacket suite.

Input
```
$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25
```

Output
```
$ hashcat -m 2100 -a 0 hash.txt rockyou.txt
<...>
$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25:ihatepasswords
<...>
```

## Transposition - Rail Fence
Flag: `Frozen chicken`

Invaders Must Die

USA, American Civil War, August 3, 1862. You are on patrol around the camp when you see an enemy rider. Once you intercepted him, you discover that he carries a message but nobody at the camp manages to uncipher it. You are the only hope to find the hidden information. It could be crucial !

Input
```
Wnb.r.ietoeh Fo"lKutrts"znl cc hi ee ekOtggsnkidy hini cna neea civo lh
```

CyberChef's Recipe
```
Rail_Fence_Cipher_Decode(8,0)
```

Output
```
Will invade Kentucky on October the eighth. signal is "Frozen chicken".
```

## Hash - NT
Flag: `iloveyou99!`

One rule to rule them all

Retrieve the password of the Administrator user from the information given by the secretsdump tool of the Impacket suite.

Input
```
aad3b435b51404eeaad3b435b51404ee:b4f79698831d92b61f886438e36c0c52
```

[Output](https://hashes.com/en/decrypt/hash)
```
b4f79698831d92b61f886438e36c0c52:iloveyou99!
```

## RSA - Factorisation
Flag: ``
