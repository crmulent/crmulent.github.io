---
title: picoCTF (General Skills) - Writeup
time: 2024-07-16 12:00:00
categories: [general]
tags: []
image: /assets/posts/picoctf/icon.svg
---

A writeup for picoGym's web challenges.

# General Skills

## Super SSH
Flag: `picoCTF{s3cur3_c0nn3ct10n_8306c99d}`

Using a Secure Shell (SSH) is going to be pretty important.

Connecting to the given server using SSH will give us the flag.

```
$ ssh -p 60181 ctf-player@titan.picoctf.net
The authenticity of host '[titan.picoctf.net]:60181 ([3.139.174.234]:60181)' can't be established.
ED25519 key fingerprint is SHA256:4S9EbTSSRZm32I+cdM5TyzthpQryv5kudRP9PIKT7XQ.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[titan.picoctf.net]:60181' (ED25519) to the list of known hosts.
ctf-player@titan.picoctf.net's password:
Welcome ctf-player, here's your flag: picoCTF{s3cur3_c0nn3ct10n_8306c99d}
Connection to titan.picoctf.net closed.
```

## endianness
Flag: `picoCTF{3ndi4n_sw4p_su33ess_817b7cfe}`

Know of little and big endian?

Solved it by learning how [endianness](https://en.wikipedia.org/wiki/Endianness) work.

```
$ nc titan.picoctf.net 58789
Welcome to the Endian CTF!
You need to find both the little endian and big endian representations of a word.
If you get both correct, you will receive the flag.
Word: vweer
Enter the Little Endian representation: 7265657776
Correct Little Endian representation!
Enter the Big Endian representation: 7677656572
Correct Big Endian representation!
Congratulations! You found both endian representations correctly!
Your Flag is: picoCTF{3ndi4n_sw4p_su33ess_817b7cfe}
```

## Commitment Issues
Flag: `picoCTF{s@n1t1z3_7246792d}`

I accidentally wrote the flag down. Good thing I deleted it!

```
$ cat message.txt
TOP SECRET

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   message.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git log --oneline
a6dca68 (HEAD -> master) remove sensitive info
e720dc2 create flag
```

I've tried checking out the previous commit but I was unsuccessful because the files would be overwritten, until I discovered there's a force flag for the checkout command.

```
$ git checkout e720dc2
error: Your local changes to the following files would be overwritten by checkout:
        message.txt
Please commit your changes or stash them before you switch branches.
Aborting

$ git checkout -f e720dc2
Note: switching to 'e720dc2'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at e720dc2 create flag
```

After successfully checking out the previous commit, I was able to view the flag.

```
$ cat message.txt
picoCTF{s@n1t1z3_7246792d}
```

## ASCII Numbers
Flag: `picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}`

Convert the following string of ASCII numbers into a readable string:

  0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d

Using CyberChef, I was able to decode the hex.
```
From_Hex('Auto')
```

## Big Zip
Flag: `picoCTF{gr3p_15_m4g1c_ef8790dc}`

Unzip this archive and find the flag.

Upon unzipping the file, I noticed theres a lot of text files and directories. So I thought I needed to iterate to all of the text files, output their content, and search for the string containing picoCTF.

```
$ find . -type f -exec cat {} \; | grep -ioE picoctf{.+}
picoCTF{gr3p_15_m4g1c_ef8790dc}
```

## First Find
Flag: `picoCTF{f1nd_15_f457_ab443fd1}`

Unzip this archive and find the file named 'uber-secret.txt'

```
$ find . -name uber-secret.txt
./adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt

$ cat $(find . -name uber-secret.txt)
picoCTF{f1nd_15_f457_ab443fd1}
```

## Serpentine
Flag: `picoCTF{7h3_r04d_l355_7r4v3l3d_aa2340b2}`

Find the flag in the Python script!

I've edited the python script to contain only the necessary functions and ran the script.

```
$ cat sol.py
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])


flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5c) + chr(0x01) + chr(0x57) + chr(0x2a) + chr(0x17) + chr(0x5e) + chr(0x5f) + chr(0x0d) + chr(0x3b) + chr(0x19) + chr(0x56) + chr(0x5b) + chr(0x5e) + chr(0x36) + chr(0x53) + chr(0x07) + chr(0x51) + chr(0x18) + chr(0x58) + chr(0x05) + chr(0x57) + chr(0x11) + chr(0x3a) + chr(0x0f) + chr(0x0a) + chr(0x5b) + chr(0x57) + chr(0x41) + chr(0x55) + chr(0x0c) + chr(0x59) + chr(0x14)


def print_flag():
  flag = str_xor(flag_enc, 'enkidu')
  print(flag)

if __name__ == "__main__":
    print_flag()
```

Running the script will give us the flag.

```
$ python3 sol.py
picoCTF{7h3_r04d_l355_7r4v3l3d_aa2340b2}
```

## Blame Game
Flag: `picoCTF{@sk_th3_1nt3rn_81e716ff}`

Someone's commits seems to be preventing the program from working. Who is it?

The challenge hints that we need to find the one that messed up the following code.

```
$ cat message.py
print("Hello, World!"
```

Running git log shows the author that pushed the code.

```
$ git log
commit 929e3918bed4c8f3432c1f7c7ddfe5b378e177e1 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:17 2024 +0000

    important business work

commit 5611f02bbfed52af8c78eb92ec58a323bebf96da
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:17 2024 +0000

    important business work

commit 455a99a92c0a2cc12675c0cf8cab5762bb7dafae
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:17 2024 +0000

    important business work
<truncated>
```

Navigating at the bottom by pressing **G** will show us the flag.

```
<truncated>
commit 80f6ecce9660dbab8e7c6e703a5dddd8fb7291ab
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:15 2024 +0000

    important business work

commit 23e9d4ce78b3cea725992a0ce6f5eea0bf0bcdd4
Author: picoCTF{@sk_th3_1nt3rn_81e716ff} <ops@picoctf.com>
Date:   Tue Mar 12 00:07:15 2024 +0000

    optimize file size of prod code

commit 3ce5c692e2f9682a866c59ac1aeae38d35d19771
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:15 2024 +0000

    create top secret project
(END)
```

## binhexa
Flag: `picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_aeaf4b09}`

How well can you perfom basic binary operations?

By opening the Windows calculator's navigation, and setting the mode the **Programmer**, we're able to solve all of the given questions.

```
$ nc titan.picoctf.net 57282

Welcome to the Binary Challenge!"
Your task is to perform the unique operations in the given order and find the final result in hexadecimal that yields the flag.

Binary Number 1: 01110100
Binary Number 2: 00110010


Question 1/6:
Operation 1: '|'
Perform the operation on Binary Number 1&2.
Enter the binary result: 1110110
Correct!

Question 2/6:
Operation 2: '&'
Perform the operation on Binary Number 1&2.
Enter the binary result: 110000
Correct!

Question 3/6:
Operation 3: '+'
Perform the operation on Binary Number 1&2.
Enter the binary result: 10100110
Correct!

Question 4/6:
Operation 4: '<<'
Perform a left shift of Binary Number 1 by 1 bits.
Enter the binary result: 11101000
Correct!

Question 5/6:
Operation 5: '*'
Perform the operation on Binary Number 1&2.
Enter the binary result: 1011010101000
Correct!

Question 6/6:
Operation 6: '>>'
Perform a right shift of Binary Number 2 by 1 bits .
Enter the binary result: 11001
Correct!

Enter the results of the last operation in hexadecimal: 19

Correct answer!
The flag is: picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_aeaf4b09}
```

## Wave a flag
Flag: `picoCTF{b1scu1ts_4nd_gr4vy_18788aaa`

Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...

Running the **file** command on the file shows that it's a Linux binary. Running the binary shows us a message that we need to use the **-h** flag. Doing so gives us the flag.

```
$ file warm
warm: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=985d9586d46e8651ab66c2fbb5a5473492466aa3, with debug_info, not stripped

$ ./warm
Hello user! Pass me a -h to learn what I can do!

$ ./warm -h
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_18788aaa}
```

## Obedient Cat
Flag: `picoCTF{s4n1ty_v3r1f13d_28e8376d}`

This file has a flag in plain sight (aka "in-the-clear").

```
$ file flag
flag: ASCII text

$ cat flag
picoCTF{s4n1ty_v3r1f13d_28e8376d}
```

## Nice netcat...
Flag: `picoCTF{g00d_k1tty!_n1c3_k1tty!_d3dfd6df}`

There is a nice program that you can talk to by using this command in a shell: $ nc mercury.picoctf.net 22902, but it doesn't speak English...

Running the command in linux outputs the following:

```
$ nc mercury.picoctf.net 22902
112
105
99
111
67
84
70
123
103
48
48
100
95
107
49
116
116
121
33
95
110
49
99
51
95
107
49
116
116
121
33
95
100
51
100
102
100
54
100
102
125
10
```

Passing the decimal values to CyberChef and converting it to ASCII gives us the flag.

```
From_Decimal('CRLF',false)
```

## Tab, Tab, Attack
Flag: `picoCTF{l3v3l_up!_t4k3_4_r35t!_d32e018c}`

Using tabcomplete in the Terminal will add years to your life, esp. when dealing with long rambling directory structures and filenames: Addadshashanammu.zip

```
$ find . -type f
./Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/Ularradallaku/fang-of-haynekhtnamet
./Addadshashanammu.zip

$ find . -type f ! -name *zip
./Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/Ularradallaku/fang-of-haynekhtnamet

$ find . -type f ! -name *zip -exec file {} \;
./Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/Ularradallaku/fang-of-haynekhtnamet: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=fcea24fb5379795a123bb860267d815e889a6d23, not stripped

$ find . -type f ! -name *zip -exec ./{} \;
*ZAP!* picoCTF{l3v3l_up!_t4k3_4_r35t!_d32e018c}
```

## Warmed Up
Flag: `picoCTF{61}`

What is 0x3D (base 16) in decimal (base 10)?

Use Windows calculator, programmer mode

## 2Warm
Flag: `picoCTF{101010}`

Can you convert the number 42 (base 10) to binary (base 2)?

## strings it
Flag: `picoCTF{5tRIng5_1T_827aee91}`

Can you find the flag in file without running it?

Running strings on the given file outputs a lot of string, hence I used grep to extract only the flag.
```
$ strings strings | grep -ioE picoctf{.+}
picoCTF{5tRIng5_1T_827aee91}
```

## First Grep
Flag: `picoCTF{grep_is_good_to_find_things_dba08a45}`

Can you find the flag in file? This would be really tedious to look through manually, something tells me there is a better way.

```
$ cat file | grep -ioE picoctf{.+}
picoCTF{grep_is_good_to_find_things_dba08a45}
```

## Bases
Flag: `picoCTF{l3arn_th3_r0p35}`

What does this bDNhcm5fdGgzX3IwcDM1 mean? I think it has something to do with bases.

Based on experience, I know it's base64. Decoding the string and enclosing it with the flag format gives us the flag.

```
$ echo bDNhcm5fdGgzX3IwcDM1 | base64 -d
l3arn_th3_r0p35
```

## plumbing
Flag: `picoCTF{digital_plumb3r_06e9d954}`

Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to jupiter.challenges.picoctf.org 7480.

Connecting to the server using netcat outputs a lot of text. So I filtered for the flag by piping the output of nc command to grep.
```
$ nc jupiter.challenges.picoctf.org 7480 | grep -i picoctf
picoCTF{digital_plumb3r_06e9d954}
```

## Based
Flag: `picoCTF{learning_about_converting_values_00a975ff}`

To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337? Connect with nc jupiter.challenges.picoctf.org 29221.

Used CyberChef's magic wand tool for every question

```
$ nc jupiter.challenges.picoctf.org 29221
Let us see how data is stored
pie
Please give the 01110000 01101001 01100101 as a word.
...
you have 45 seconds.....

Input:
pie
Please give me the  156 165 162 163 145 as a word.
Input:
nurse
Please give me the 6c69676874 as a word.
Input:
light
You've beaten the challenge
Flag: picoCTF{learning_about_converting_values_00a975ff}
```

## what's a net cat?
Flag: `picoCTF{nEtCat_Mast3ry_284be8f7}`

```
$ nc jupiter.challenges.picoctf.org 64287
You're on your way to becoming the net cat master
picoCTF{nEtCat_Mast3ry_284be8f7}
```

## Lets Warm Up
Flag: `picoCTF{p}`

If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?

Used CyberChef
```
From_Hex('0x')
```

## mus1c
Flag: ``
I wrote you a song. Put it in the picoCTF{} flag format.

## flag_shop
Flag: `picoCTF{m0n3y_bag5_9c5fac9b}`

There's a flag shop selling stuff, can you buy a flag? Source. Connect with nc jupiter.challenges.picoctf.org 4906.

Inspecting the code, we need to overflow the **number_flags** variable to be able to afford the real flag.

```
$ nc jupiter.challenges.picoctf.org 4906
Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
1



 Balance: 1100


Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
2
Currently for sale
1. Defintely not the flag Flag
2. 1337 Flag
1
These knockoff Flags cost 900 each, enter desired quantity
3333333

The final cost is: -1294967596

Your current balance after transaction: 1294968696

Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
2
Currently for sale
1. Defintely not the flag Flag
2. 1337 Flag
2
1337 flags cost 100000 dollars, and we only have 1 in stock
Enter 1 to buy one1
YOUR FLAG IS: picoCTF{m0n3y_bag5_9c5fac9b}
```

## 1_wanna_b3_a_r0ck5tar
Flag: ``

I wrote you another song. Put the flag in the picoCTF{} flag format

## Magikarp Ground Mission
Flag: `picoCTF{xxsh_0ut_0f_\/\/4t3r_21cac893}`

Do you know how to move between directories and read files in the shell? Start the container, `ssh` to it, and then `ls` once connected to begin. Login via `ssh` as `ctf-player` with the password, `abcba9f7`

```
ctf-player@pico-chall$ ls
1of3.flag.txt  instructions-to-2of3.txt

ctf-player@pico-chall$ cat 1of3.flag.txt
picoCTF{xxsh_

ctf-player@pico-chall$ cat instructions-to-2of3.txt
Next, go to the root of all things, more succinctly `/`

ctf-player@pico-chall$ cd / && ls
2of3.flag.txt  bin  boot  dev  etc  home  instructions-to-3of3.txt  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

ctf-player@pico-chall$ cat 2of3.flag.txt
0ut_0f_\/\/4t3r_

ctf-player@pico-chall$ cat instructions-to-3of3.txt
Lastly, ctf-player, go home... more succinctly `~`

ctf-player@pico-chall$ cd ~ && ls
3of3.flag.txt  drop-in

ctf-player@pico-chall$ cat 3of3.flag.txt
21cac893}
```

## Python Wrangling
Flag: `picoCTF{4p0110_1n_7h3_h0us3_dbd1bea4}`

Python scripts are invoked kind of like programs in the Terminal... Can you run this Python script using this password to get the flag?

```
$ python3 ende.py -d flag.txt.en < pw.txt
Please enter the password:picoCTF{4p0110_1n_7h3_h0us3_dbd1bea4}
```

## Static ain't always noise
Flag: `picoCTF{d15a5m_t34s3r_ccb2b43e}`

Can you look at the data in this binary: static? This BASH script might help!

```
$ strings static | grep pico
picoCTF{d15a5m_t34s3r_ccb2b43e}
```

## runme.py
Flag: `picoCTF{run_s4n1ty_run}`

Run the runme.py script to get the flag. Download the script with your browser or with wget in the webshell.

```
$ python3 runme.py
picoCTF{run_s4n1ty_run}
```

## PW Crack 1
Flag: `picoCTF{545h_r1ng1ng_1b2fd683}`

Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag in the same directory too.

Inspecting the python file, the password is **8713**

```
<truncated>
def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "8713"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")
<truncated>
```

```
$ echo 8713 | python3 level1.py
Please enter correct password for flag: Welcome back... your flag, user:
picoCTF{545h_r1ng1ng_1b2fd683}
```

## PW Crack 2
Flag: `picoCTF{tr45h_51ng1ng_9701e681}`

Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag in the same directory too.

Decode the password in the python file using the python interpreter.
```
>>> chr(0x34) + chr(0x65) + chr(0x63) + chr(0x39)
'4ec9'
```

```
$ echo 4ec9 | python3 level2.py
Please enter correct password for flag: Welcome back... your flag, user:
picoCTF{tr45h_51ng1ng_9701e681}
```

## PW Crack 3
Flag: `picoCTF{m45h_fl1ng1ng_6f98a49f}`

Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. There are 7 potential passwords with 1 being correct. You can find these by examining the password checker script.

Modified the original python code to loop through all of the possible passwords.

```
def level_3_pw_check(n):
    user_pw = n
    user_pw_hash = hash_pw(user_pw)

    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

# The strings below are 7 possibilities for the correct password.
#   (Only 1 is correct)
pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]

[level_3_pw_check(i) for i in pos_pw_list]
```

Running the code will give us the flag.
```
$ python3 sol.py | grep pico
picoCTF{m45h_fl1ng1ng_6f98a49f}
```

## PW Crack 4
Flag: `picoCTF{fl45h_5pr1ng1ng_cf341ff1}`
Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. There are 100 potential passwords with only 1 being correct. You can find these by examining the password checker script.

Same as before, modify the script and run it.
```
def level_4_pw_check(n):
    user_pw = n
    user_pw_hash = hash_pw(user_pw)

    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

# The strings below are 100 possibilities for the correct password.
#   (Only 1 is correct)
pos_pw_list = ["158f", "1655", "d21e", "4966", "ed69", "1010", "dded", "844c", "40ab", "a948", "156c", "ab7f", "4a5f", "e38c", "ba12", "f7fd", "d780", "4f4d", "5ba1", "96c5", "55b9", "8a67", "d32b", "aa7a", "514b", "e4e1", "1230", "cd19", "d6dd", "b01f", "fd2f", "7587", "86c2", "d7b8", "55a2", "b77c", "7ffe", "4420", "e0ee", "d8fb", "d748", "b0fe", "2a37", "a638", "52db", "51b7", "5526", "40ed", "5356", "6ad4", "2ddd", "177d", "84ae", "cf88", "97a3", "17ad", "7124", "eff2", "e373", "c974", "7689", "b8b2", "e899", "d042", "47d9", "cca9", "ab2a", "de77", "4654", "9ecb", "ab6e", "bb8e", "b76b", "d661", "63f8", "7095", "567e", "b837", "2b80", "ad4f", "c514", "ffa4", "fc37", "7254", "b48b", "d38b", "a02b", "ec6c", "eacc", "8b70", "b03e", "1b36", "81ff", "77e4", "dbe6", "59d9", "fd6a", "5653", "8b95", "d0e5"]

[level_4_pw_check(i) for i in pos_pw_list]
```

```
$ python3 sol.py | grep pico
picoCTF{fl45h_5pr1ng1ng_cf341ff1}
```

## PW Crack 5
Flag: `picoCTF{h45h_sl1ng1ng_fffcda23}`

Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. Here's a dictionary with all possible passwords based on the password conventions we've seen so far.

Modified python code to read **dictionary.txt** and pass each password to the password checker.

```
def level_5_pw_check(n):
    user_pw = n
    user_pw_hash = hash_pw(user_pw)

    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

with open("dictionary.txt", "r") as f:
    pwds = f.readlines()

    [level_5_pw_check(i.strip()) for i in pwds]
```

```
$ python3 sol.py | grep pico
picoCTF{h45h_sl1ng1ng_fffcda23}
```

## convertme.py
Flag: `picoCTF{4ll_y0ur_b4535_9c3b7d4d}`

Run the Python script and convert the given number from decimal to binary to get the flag.

Delete unnecessary lines of code and run the file.

```
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])


flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5f) + chr(0x05) + chr(0x08) + chr(0x2a) + chr(0x1c) + chr(0x5e) + chr(0x1e) + chr(0x1b) + chr(0x3b) + chr(0x17) + chr(0x51) + chr(0x5b) + chr(0x58) + chr(0x5c) + chr(0x3b) + chr(0x4c) + chr(0x06) + chr(0x5d) + chr(0x09) + chr(0x5e) + chr(0x00) + chr(0x41) + chr(0x01) + chr(0x13)


flag = str_xor(flag_enc, 'enkidu')
print('That is correct! Here\'s your flag: ' + flag)
```

```
$ python3 sol.py
That is correct! Here's your flag: picoCTF{4ll_y0ur_b4535_9c3b7d4d}
```

## Codebook
Flag: `picoCTF{c0d3b00k_455157_7d102d7a}`

Run the Python script code.py in the same directory as codebook.txt.

self explanatory
```
$ python3 code.py
picoCTF{c0d3b00k_455157_7d102d7a}
```

## fixme1.py
Flag: `picoCTF{1nd3nt1ty_cr1515_6a476c8f}`

Fix the syntax error in this Python script to print the flag.

Remove unnecessary indentation and run the file.

```
$ python3 fixme1.py
  File "/mnt/d/ctf/pico/General Skills/fixme1.py/fixme1.py", line 18
    print('That is correct! Here\'s your flag: ' + flag)
IndentationError: unexpected indent
```

```
$ python3 fixme1.py
That is correct! Here's your flag: picoCTF{1nd3nt1ty_cr1515_6a476c8f}
```

## fixme2.py
Flag: `picoCTF{3qu4l1ty_n0t_4551gnm3nt_f6a5aefc}`

Fix the syntax error in the Python script to print the flag.

Changed the assignment operatr to equality operator.
```
$ python3 fixme2.py
  File "/mnt/d/ctf/pico/General Skills/fixme2.py/fixme2.py", line 22
    if flag = "":
       ^^^^^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

```
$ python3 fixme2.py
That is correct! Here's your flag: picoCTF{3qu4l1ty_n0t_4551gnm3nt_f6a5aefc}
```

## Glitch Cat
Flag: `picoCTF{gl17ch_m3_n07_a4392d2e}`

Our flag printing service has started glitching!

Use python to evaluate the expression.
```
$ nc saturn.picoctf.net 55200
'picoCTF{gl17ch_m3_n07_' + chr(0x61) + chr(0x34) + chr(0x33) + chr(0x39) + chr(0x32) + chr(0x64) + chr(0x32) + chr(0x65) + '}'

$ python3
Python 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 'picoCTF{gl17ch_m3_n07_' + chr(0x61) + chr(0x34) + chr(0x33) + chr(0x39) + chr(0x32) + chr(0x64) + chr(0x32) + chr(0x65) + '}'
'picoCTF{gl17ch_m3_n07_a4392d2e}'
```

## HashingJobApp
Flag: `picoCTF{4ppl1c4710n_r3c31v3d_3eb82b73}`

If you want to hash with the best, beat this test!

```
$ nc saturn.picoctf.net 51042
Please md5 hash the text between quotes, excluding the quotes: 'crawl space'
Answer:
a320efce66b14e02f4569a08959ad2e8
a320efce66b14e02f4569a08959ad2e8
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'Cinco de Mayo'
Answer:
3b55fa8f34aae6b045ccec7f9b5d4c89
3b55fa8f34aae6b045ccec7f9b5d4c89
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'bad dogs'
Answer:
60cc96ffdc458c98395d6e7b6878a6e9
60cc96ffdc458c98395d6e7b6878a6e9
Correct.
picoCTF{4ppl1c4710n_r3c31v3d_3eb82b73}
```

```
$ echo -n 'crawl space' | md5sum
a320efce66b14e02f4569a08959ad2e8  -

$ echo -n 'Cinco de Mayo' | md5sum
3b55fa8f34aae6b045ccec7f9b5d4c89  -

$ echo -n 'bad dogs' | md5sum
60cc96ffdc458c98395d6e7b6878a6e9  -
```

## repetitions
Flag: `picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_3f81f7be}`

Can you make sense of this file?

```
$ cat enc_flag
VmpGU1EyRXlUWGxTYmxKVVYwZFNWbGxyV21GV1JteDBUbFpPYWxKdFVsaFpWVlUxWVZaS1ZWWnVh
RmRXZWtab1dWWmtSMk5yTlZWWApiVVpUVm10d1VWZFdVa2RpYlZaWFZtNVdVZ3BpU0VKeldWUkNk
MlZXVlhoWGJYQk9VbFJXU0ZkcVRuTldaM0JZVWpGS2VWWkdaSGRXCk1sWnpWV3hhVm1KRk5XOVVW
VkpEVGxaYVdFMVhSbFZhTTBKUFdXdGtlbVF4V2tkWGJYUllDbUY2UWpSWmEyaFRWakpHZEdWRlZs
aGkKYlRrelZERldUMkpzUWxWTlJYTkxDZz09Cg==

$ cat enc_flag | base64 -d
VjFSQ2EyTXlSblJUV0dSVllrWmFWRmx0TlZOalJtUlhZVVU1YVZKVVZuaFdWekZoWVZkR2NrNVVX
bUZTVmtwUVdWUkdibVZXVm5WUgpiSEJzWVRCd2VWVXhXbXBOUlRWSFdqTnNWZ3BYUjFKeVZGZHdW
MlZzVWxaVmJFNW9UVVJDTlZaWE1XRlVaM0JPWWtkemQxWkdXbXRYCmF6QjRZa2hTVjJGdGVFVlhi
bTkzVDFWT2JsQlVNRXNLCg==

$ cat enc_flag | base64 -d | base64 -d
V1RCa2MyRnRTWGRVYkZaVFltNVNjRmRXYUU5aVJUVnhWVzFhYVdGck5UWmFSVkpQWVRGbmVWVnVR
bHBsYTBweVUxWmpNRTVHWjNsVgpXR1JyVFdwV2VsUlZVbE5oTURCNVZXMWFUZ3BOYkdzd1ZGWmtX
azB4YkhSV2FteEVXbm93T1VOblBUMEsK

$ cat enc_flag | base64 -d | base64 -d | base64 -d
WTBkc2FtSXdUbFZTYm5ScFdWaE9iRTVxVW1aaWFrNTZaRVJPYTFneVVuQlpla0pyU1ZjME5GZ3lV
WGRrTWpWelRVUlNhMDB5VW1aTgpNbGswVFZkWk0xbHRWamxEWnowOUNnPT0K

$ cat enc_flag | base64 -d | base64 -d | base64 -d | base64 -d
Y0dsamIwTlVSbnRpWVhObE5qUmZiak56ZEROa1gyUnBZekJrSVc0NFgyUXdkMjVzTURSa00yUmZN
Mlk0TVdZM1ltVjlDZz09Cg==

$ cat enc_flag | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
cGljb0NURntiYXNlNjRfbjNzdDNkX2RpYzBkIW44X2Qwd25sMDRkM2RfM2Y4MWY3YmV9Cg==

$ cat enc_flag | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_3f81f7be}
```

## Time Machine
Flag: `picoCTF{t1m3m@ch1n3_e8c98b3a}`

What was I last working on? I remember writing a note to help me remember...

```
$ cat message.txt
This is what I was working on, but I'd need to look at my commit history to know why...

$ git log
commit 712314f105348e295f8cadd7d7dc4e9fa871e9a2 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:26 2024 +0000

    picoCTF{t1m3m@ch1n3_e8c98b3a}
```

## Binary Search
Flag: `picoCTF{g00d_gu355_ee8225d0}`

Want to play a game? As you use more of the shell, you might be interested in how they work! Binary search is a classic algorithm used to quickly find an item in a sorted list. Can you find the flag? You'll have 1000 possibilities and only 10 guesses. Cyber security often has a huge amount of data to look through - from logs, vulnerability reports, and forensics. Practicing the fundamentals manually might help you in the future when you have to write your own tools!

```
Welcome to the Binary Search Game!
I'm thinking of a number between 1 and 1000.
Enter your guess: 500
Higher! Try again.
Enter your guess: 750
Lower! Try again.
Enter your guess: 625
Lower! Try again.
Enter your guess: 565
Higher! Try again.
Enter your guess: 595
Higher! Try again.
Enter your guess: 610
Higher! Try again.
Enter your guess: 617
Higher! Try again.
Enter your guess: 620
Higher! Try again.
Enter your guess: 623
Lower! Try again.
Enter your guess: 622
Congratulations! You guessed the correct number: 622
Here's your flag: picoCTF{g00d_gu355_ee8225d0}
Connection to atlas.picoctf.net closed.
```

## Collaborative Development
Flag: `picoCTF{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_6c06cec1}`

```
$ cat flag.py
print("Printing the flag...")

$ git log
commit 2258a0f267d57e8b6025e2a020b77fac7a553c92 (HEAD -> main)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:54 2024 +0000

    init flag printer

$ git branch
  feature/part-1
  feature/part-2
  feature/part-3
* main

$ git checkout feature/part-1 -f && cat flag.py
Switched to branch 'feature/part-1'
print("Printing the flag...")
print("picoCTF{t3@mw0rk_", end='')

$ git checkout feature/part-2 -f && cat flag.py
Switched to branch 'feature/part-2'
print("Printing the flag...")

print("m@k3s_th3_dr3@m_", end='')

$ git checkout feature/part-3 -f && cat flag.py
Switched to branch 'feature/part-3'
print("Printing the flag...")

print("w0rk_6c06cec1}")
```
