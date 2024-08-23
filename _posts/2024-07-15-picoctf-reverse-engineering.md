---
title: picoCTF (Reverse Engineering) - Writeup
time: 2024-07-16 12:00:00
categories: [reverse engineering]
tags: []
image: /assets/posts/picoctf/icon.svg
---

A writeup for picoGym's reverse engineering challenges.

# Reverse Engineering

## format string 0
Flag: `picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_74f6c0e7}`
Can you use your knowledge of format strings to make the customers happy?

Copy the string in the C file as input.
```
$ nc mimas.picoctf.net 60328
Welcome to our newly-opened burger place Pico 'n Patty! Can you help the picky customers find their favorite burger?
Here comes the first customer Patrick who wants a giant bite.
Please choose from the following burgers: Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
Enter your recommendation: Gr%114d_Cheese
Gr                                                                                                           4202954_Cheese
Good job! Patrick is happy! Now can you serve the second customer?
Sponge Bob wants something outrageous that would break the shop (better be served quick before the shop owner kicks you out!)
Please choose from the following burgers: Pe%to_Portobello, $outhwest_Burger, Cla%sic_Che%s%steak
Enter your recommendation: Cla%sic_Che%s%steak
ClaCla%sic_Che%s%steakic_Che(null)
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_74f6c0e7}
```

## heap 0
Flag: `picoCTF{my_first_heap_overflow_4fa6dd49}`

Are overflows just a stack concern?

```
$ nc tethys.picoctf.net 60421


Welcome to heap0!
I put my data on the heap so it should be safe from any tampering.
Since my data isn't on the stack I'll even let you write whatever info you want to the heap, I already took care of using malloc for you.

Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data
+-------------+----------------+
[*]   0x611168c192b0  ->   pico
+-------------+----------------+
[*]   0x611168c192d0  ->   bico
+-------------+----------------+

1. Print Heap:          (print the current state of the heap)
2. Write to buffer:     (write to your own personal block of data on the heap)
3. Print safe_var:      (I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:          (Try to print the flag, good luck)
5. Exit

Enter your choice: 2
Data for buffer: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa

1. Print Heap:          (print the current state of the heap)
2. Write to buffer:     (write to your own personal block of data on the heap)
3. Print safe_var:      (I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:          (Try to print the flag, good luck)
5. Exit

Enter your choice: 4

YOU WIN
picoCTF{my_first_heap_overflow_4fa6dd49}
```

## heap 1
Flag: `picoCTF{starting_to_get_the_hang_79ee3270}`

Can you control your overflow?

```
$ nc tethys.picoctf.net 56995

Welcome to heap1!
I put my data on the heap so it should be safe from any tampering.
Since my data isn't on the stack I'll even let you write whatever info you want to the heap, I already took care of using malloc for you.

Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data
+-------------+----------------+
[*]   0x57bf98ca92b0  ->   pico
+-------------+----------------+
[*]   0x57bf98ca92d0  ->   bico
+-------------+----------------+

1. Print Heap:          (print the current state of the heap)
2. Write to buffer:     (write to your own personal block of data on the heap)
3. Print safe_var:      (I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:          (Try to print the flag, good luck)
5. Exit

Enter your choice: 2
Data for buffer: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaapico

1. Print Heap:          (print the current state of the heap)
2. Write to buffer:     (write to your own personal block of data on the heap)
3. Print safe_var:      (I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:          (Try to print the flag, good luck)
5. Exit

Enter your choice: 1
4Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data
+-------------+----------------+
[*]   0x57bf98ca92b0  ->   aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaapico
+-------------+----------------+
[*]   0x57bf98ca92d0  ->   pico
+-------------+----------------+

1. Print Heap:          (print the current state of the heap)
2. Write to buffer:     (write to your own personal block of data on the heap)
3. Print safe_var:      (I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:          (Try to print the flag, good luck)
5. Exit

Enter your choice:

YOU WIN
picoCTF{starting_to_get_the_hang_79ee3270}
```

# Reverse

## vault-door-training
Flag: `picoCTF{w4rm1ng_Up_w1tH_jAv4_eec0716b713}`

Your mission is to enter Dr. Evil's laboratory and retrieve the blueprints for his Doomsday Project. The laboratory is protected by a series of locked vault doors. Each door is controlled by a computer and requires a password to open. Unfortunately, our undercover agents have not been able to obtain the secret passwords for the vault doors, but one of our junior agents obtained the source code for each vault's computer! You will need to read the source code for each level to figure out what the password is for that vault door. As a warmup, we have created a replica vault in our training facility. The source code for the training vault is here: VaultDoorTraining.java

The vault's password is hardcoded into the source code. Enclosing the password with the flag format grants us access to the vault.

```
$ javac VaultDoorTraining.java && java VaultDoorTraining
Enter vault password: picoCTF{w4rm1ng_Up_w1tH_jAv4_eec0716b713}
Access granted.
```

## GDB baby step 1
Flag: `picoCTF{549698}`

Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

```
$ gdb ./debugger0_a

(gdb) lay next
(gdb) break *main+21
Breakpoint 1 at 0x113e
(gdb) run
(gdb) info regsiters eax
```

## GDB baby step 2
Flag: `picoCTF{307019}`

Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

```
$ gdb ./debugger0_b

(gdb) lay next
```

[pic1]

Inspecting the assembly, we can see that the main ends with address 0x401142 or *main+60. We can set a breakpoint at that pointer.

[pic2]

[pic3]

## GDB baby step 3
Flag: ``

Now for something a little different. 0x2262c96b is loaded into memory in the main function. Examine byte-wise the memory that the constant is loaded in by using the GDB command x/4xb addr. The flag is the four bytes as they are stored in memory. If you find the bytes 0x11 0x22 0x33 0x44 in the memory location, your flag would be: picoCTF{0x11223344}.

## ASCII FTW
Flag: `picoCTF{ASCII_IS_EASY_7BCD971D}`

This program has constructed the flag using hex ascii values. Identify the flag text by disassembling the program.

Running the program outputs a message that the flag starts with 70 (hex). Debugging the program using gdb and disassembling main outputs the assembly instructions. I noticed a series of hex values that starts with 70. Extracting all of that values and putting in to CyberChef gives us the flag. 

Extracted hex: **7069636f4354467b41534349495f49535f454153595f37424344393731447d**
```
$ ./asciiftw
The flag starts with 70

$ gdb ./asciiftw
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./asciiftw...
(No debugging symbols found in ./asciiftw)
(gdb) disass main
Dump of assembler code for function main:
   0x0000000000001169 <+0>:     endbr64
   0x000000000000116d <+4>:     push   %rbp
   0x000000000000116e <+5>:     mov    %rsp,%rbp
   0x0000000000001171 <+8>:     sub    $0x30,%rsp
   0x0000000000001175 <+12>:    mov    %fs:0x28,%rax
   0x000000000000117e <+21>:    mov    %rax,-0x8(%rbp)
   0x0000000000001182 <+25>:    xor    %eax,%eax
   0x0000000000001184 <+27>:    movb   $0x70,-0x30(%rbp)
   0x0000000000001188 <+31>:    movb   $0x69,-0x2f(%rbp)
   0x000000000000118c <+35>:    movb   $0x63,-0x2e(%rbp)
   0x0000000000001190 <+39>:    movb   $0x6f,-0x2d(%rbp)
   0x0000000000001194 <+43>:    movb   $0x43,-0x2c(%rbp)
   0x0000000000001198 <+47>:    movb   $0x54,-0x2b(%rbp)
   0x000000000000119c <+51>:    movb   $0x46,-0x2a(%rbp)
   0x00000000000011a0 <+55>:    movb   $0x7b,-0x29(%rbp)
   0x00000000000011a4 <+59>:    movb   $0x41,-0x28(%rbp)
   0x00000000000011a8 <+63>:    movb   $0x53,-0x27(%rbp)
   0x00000000000011ac <+67>:    movb   $0x43,-0x26(%rbp)
   0x00000000000011b0 <+71>:    movb   $0x49,-0x25(%rbp)
   0x00000000000011b4 <+75>:    movb   $0x49,-0x24(%rbp)
   0x00000000000011b8 <+79>:    movb   $0x5f,-0x23(%rbp)
   0x00000000000011bc <+83>:    movb   $0x49,-0x22(%rbp)
   0x00000000000011c0 <+87>:    movb   $0x53,-0x21(%rbp)
   0x00000000000011c4 <+91>:    movb   $0x5f,-0x20(%rbp)
   0x00000000000011c8 <+95>:    movb   $0x45,-0x1f(%rbp)
   0x00000000000011cc <+99>:    movb   $0x41,-0x1e(%rbp)
   0x00000000000011d0 <+103>:   movb   $0x53,-0x1d(%rbp)
   0x00000000000011d4 <+107>:   movb   $0x59,-0x1c(%rbp)
   0x00000000000011d8 <+111>:   movb   $0x5f,-0x1b(%rbp)
   0x00000000000011dc <+115>:   movb   $0x37,-0x1a(%rbp)
   0x00000000000011e0 <+119>:   movb   $0x42,-0x19(%rbp)
   0x00000000000011e4 <+123>:   movb   $0x43,-0x18(%rbp)
   0x00000000000011e8 <+127>:   movb   $0x44,-0x17(%rbp)
   0x00000000000011ec <+131>:   movb   $0x39,-0x16(%rbp)
   0x00000000000011f0 <+135>:   movb   $0x37,-0x15(%rbp)
   0x00000000000011f4 <+139>:   movb   $0x31,-0x14(%rbp)
   0x00000000000011f8 <+143>:   movb   $0x44,-0x13(%rbp)
   0x00000000000011fc <+147>:   movb   $0x7d,-0x12(%rbp)
   0x0000000000001200 <+151>:   movzbl -0x30(%rbp),%eax
   0x0000000000001204 <+155>:   movsbl %al,%eax
```

## file-run1
Flag: `picoCTF{U51N6_Y0Ur_F1r57_F113_47cf2b7b}`

A program has been provided to you, what happens if you try to run it on the command line?

Simply run the file in shell.

```
$ ./run
The flag is: picoCTF{U51N6_Y0Ur_F1r57_F113_47cf2b7b}
```

## file-run2
Flag: `picoCTF{F1r57_4rgum3n7_f65ed63e}`

Another program, but this time, it seems to want some input. What happens if you try to run it on the command line with input "Hello!"?

Running it the first time gave us a message that we need to run it with an argument. Passing a random argument says that we specifically need to pass the **Hello!** string as an argument. Doing so will give us the flag.

```
$ ./run
Run this file with only one argument.

$ ./run test
Won't you say 'Hello!' to me first?

$ ./run Hello!
The flag is: picoCTF{F1r57_4rgum3n7_f65ed63e}
```
