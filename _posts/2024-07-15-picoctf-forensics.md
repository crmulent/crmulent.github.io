---
title: picoCTF (Forensics) - Writeup
time: 2024-07-16 12:00:00
categories: [forensics]
tags: []
image: /assets/posts/picoctf/icon.svg
---

A writeup for picoGym's forensic challenges.

# Forensics

## Verify
Flag: `picoCTF{trust_but_verify_c6c8b911}`
People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.

```
$ ssh -p 50605 ctf-player@rhea.picoctf.net
```

```
ctf-player@pico-chall$ find files -type f -exec ./decrypt.sh {} \; 2>/dev/null | grep -oiE picoctf{.+}
```

## Scan Surprise
Flag: `picoCTF{p33k_@_b00_3f7cf1ae}`
I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?

```
zbarimg flag.png 2>/dev/null | grep -oiE picoctf{.+}
```

## Secret of the Polyglot
Flag: `picoCTF{f1u3n7_1n_pn9_&_pdf_724b1287}`

The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?

Opening the pdf file shows the 2nd part of the flag. To find the other flag, I ran the file command on the file and found out that it's a valid PNG file. Copying the file with a PNG file extension reveals the other half of the flag.
```
$ file *
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
$ cp flag2of2-final.pdf image.png
```

## CanYouSee
Flag: `picoCTF{ME74D47A_HIDD3N_deca06fb}`

How about some hide and seek?

Opening the image doesn't show the flag so I ran exiftool on the image and found a base64 code. Decoding the code reveals the flag.

```
$ exiftool ukn_reality.jpg
ExifTool Version Number         : 12.40
File Name                       : ukn_reality.jpg
Directory                       : .
File Size                       : 2.2 MiB
File Modification Date/Time     : 2024:02:16 06:40:14+08:00
File Access Date/Time           : 2024:07:15 17:55:25+08:00
File Inode Change Date/Time     : 2024:07:15 17:55:25+08:00
File Permissions                : -rwxrwxrwx
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 72
Y Resolution                    : 72
XMP Toolkit                     : Image::ExifTool 11.88
Attribution URL                 : cGljb0NURntNRTc0RDQ3QV9ISUREM05fZGVjYTA2ZmJ9Cg==
Image Width                     : 4308
Image Height                    : 2875
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 4308x2875
Megapixels                      : 12.4

$ echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fZGVjYTA2ZmJ9Cg==" | base64 -d
picoCTF{ME74D47A_HIDD3N_deca06fb}
```

## PcapPoisoning
Flag: `picoCTF{P64P_4N4L7S1S_SU55355FUL_4624a8b6}`

How about some hide and seek heh?

Specting the pcap file using wireshark doesn't do so much so I ran the strings command on the file and luckily I got the flag.

```
$ strings trace.pcap | grep -ioE picoctf{.+}
picoCTF{P64P_4N4L7S1S_SU55355FUL_4624a8b6}
```

## hideme
Flag: `picoCTF{Hiddinng_An_imag3_within_@n_ima9e_d55982e8}`

Every file gets a flag.
The SOC analyst saw one image been sent back and forth between two people. They decided to investigate and found out that there was more than what meets the eye here.

Running strings on the file, I noticed that it contains a hidden png file inside it. Running unzip command on the file reveals the flag.

```
$ strings flag.png | tail
Lr/q
#SS_
ecd54
{)II
UTLDk
})++
-^Mrr0
QPPWP
secret/UT
secret/flag.pngUT

$ unzip flag.png
Archive:  flag.png
warning [flag.png]:  39739 extra bytes at beginning or within zipfile
  (attempting to process anyway)
   creating: secret/
  inflating: secret/flag.png
```

## FindAndOpen
Flag: `picoCTF{R34DING_LOKd_fil56_succ3ss_494c4f32}`

Someone might have hidden the password in the trace file.
Find the key to unlock this file. This tracefile might be good to analyze.

We are given a pcap and a zip file. Opening the pcap file, I noticed that the packets contains a lot of base64 code. So I ran strings command on the pcap file, sorting it, and getting the uniq values to find uncommon base64 code. Decoding the longest base64 code reveals the password for the zip file. Extracting the zip file with the reveals the flag.

```
$ strings dump.pcap | sort | uniq
#id=18e2a8da30459b730aec93a71af19988#cd=EB3B62CAFB4F51F0114CFB58C4FE2C2F
+Chromecast-18e2a8da30459b730aec93a71af19988
.+Chromecast-18e2a8da30459b730aec93a71af19988
AABBHHPJGTFRLKVGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8=
Flying on Ethernet secret: Is this the flag
I$18e2a8da-3045-9b73-0aec-93a71af19988
PBwaWUvQ1RGe1Maybe try checking the other file
PBwaWUvQ1RGesabababkjaASKBKSBACVVAVSDDSSSSDSKJBJS
ScTo
_googlecast
_tcp
bs=FA8FCA54567B
fn=Living Room TV       ca=465413
iBwaWNvQ1RGe1Could the flag have been splitted?
ic=/setup/icon.png
local
md=Chromecast
nf=2
rm=B8B7BA5A0CEB76E5
st=0
ve=05

$ echo "VGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8=" | base64 -d
This is the secret: picoCTF{R34DING_LOKd_

$ unzip -P "picoCTF{R34DING_LOKd_" flag.zip
Archive:  flag.zip
 extracting: flag

$ cat flag
picoCTF{R34DING_LOKd_fil56_succ3ss_494c4f32}
```

## St3g0
Flag: `picoCTF{7h3r3_15_n0_5p00n_96ae0ac1}`

Download this image and find the flag.

Running strings, exiftool, and binwalk on the file doesn't reveal flag. Suddenly I remembered that you can hide data using the LSB approach. With that I ran the the zsteg command and got the flag.

```
$ zsteg pico.flag.png | grep -ioE picoctf{.+}
picoCTF{7h3r3_15_n0_5p00n_96ae0ac1}
```

## Sleuthkit Intro
Flag: `picoCTF{mm15_f7w!}`

Download the disk image and use mmls on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.
Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.

```
$ mmls disk.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)

$ echo "202752" | nc saturn.picoctf.net 59234
202752
What is the size of the Linux partition in the given disk image?
Length in sectors: Great work!
picoCTF{mm15_f7w!}
```

## Redaction gone wrong
Flag: `picoCTF{C4n_Y0u_S33_m3_fully}`

Now you DON’T see me.
This report has some critical data in it, some of which have been redacted correctly, while some were not. Can you find an important key that was not redacted properly?

Opening the file, we can see that some of the text are redacted. But I noticed that you can still select the text that has been removed. So I copied the contents of the pdf and it shows the flag.

![1](/assets/posts/picoctf/forensics/1.png)

```
Financial Report for ABC Labs, Kigali, Rwanda for the year 2021.
Breakdown - Just painted over in MS word.

Cost Benefit Analysis
Credit Debit
This is not the flag, keep looking
Expenses from the
picoCTF{C4n_Y0u_S33_m3_fully}
Redacted document.
```

## Packets Primer
Flag: `picoCTF{p4ck37_5h4rk_ceccaa7f}`

Download the packet capture file and use packet analysis software to find the flag.

Inspecting the packets, I noticed that the flag is in the data of the packet no. 4. But each character of the flag is delimited a space so I extracted the flag using tshark to automate the removal of spaces.

![2](/assets/posts/picoctf/forensics/2.png)

```
$ tshark -r network-dump.flag.pcap -Tfields -edata.data | xxd -r -p | tr -d ' '
picoCTF{p4ck37_5h4rk_ceccaa7f}
```

## Lookey here
Flag: `picoCTF{gr3p_15_@w3s0m3_4c479940}`

Attackers have hidden information in a very large mass of data in the past, maybe they are still doing it.

Opening the file, I noticed that it contains a lot of text so I use grep command to find the flag in the file.

```
$ grep -ioE picoctf{.+} anthem.flag.txt
picoCTF{gr3p_15_@w3s0m3_4c479940}
```

## File types
Flag: 

This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.

## Enhance!
Flag: `picoCTF{3nh4nc3d_d0a757bf}`

Download this image file and find the flag.

Calling strings on the file, I noticed that the flag is inside span the span tag. So I used string manipulation to extract the flag.

```
$ strings drawing.flag.svg | tail
         y="132.11147"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3764">F { 3 n h 4 n </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.11588"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3752">c 3 d _ d 0 a 7 5 7 b f }</tspan></text>
  </g>
</svg>
```

```
$ strings drawing.flag.svg | grep span3 | cut -d'>' -f2 | cut -d'<' -f1 | tr '\n' ' ' | tr -d ' ' && echo
picoCTF{3nh4nc3d_d0a757bf}
```

## information
Flag: `picoCTF{the_m3tadata_1s_modified}`

Files can always be changed in a secret way. Can you find the flag? cat.jpg

Running exiftool on the image, I noticed that there's a base64 code as it's license. Decoding the code outputs the flag.

```
$ echo 'cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9' | base64 -d
picoCTF{the_m3tadata_1s_modified}
```

## Disk, disk, sleuth! II
Flag: ``

All we know is the file with the flag is named `down-at-the-bottom.txt`... Disk image: dds2-alpine.flag.img.gz


## MacroHard WeakEdge
Flag: ``

I've hidden a flag in this file. Can you find it? Forensics is fun.pptm


## Matryoshka doll
Flag: ``

Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: this

```
$ strings dolls.jpg | tail
6~vz
\3 @
q.*     f%
Og\*8
?41v57
4wed34e
` 'h
pR}tt
base_images/2_c.jpgUT
O`ux

$ unzip dolls.jpg
Archive:  dolls.jpg
warning [dolls.jpg]:  272492 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: base_images/2_c.jpg

$ ls
base_images  dolls.jpg

$ cd base_images/ && ls
2_c.jpg

$ strings 2_c.jpg | tail
k%kZW
R1m_
Uprg
U[b*
_[o/55
D$-|
base_images/4_c.jpgUT
O`ux
base_images/3_c.jpgUT
O`ux

$ unzip 2_c.jpg
Archive:  2_c.jpg
warning [2_c.jpg]:  187707 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: base_images/3_c.jpg

$ cd base_images/ && ls
3_c.jpg

$ strings 3_c.jpg | tail
R4RFW
0~}0\
k%kZW
R1m_
Uprg
U[b*
_[o/55
D$-|
base_images/4_c.jpgUT
O`ux

$ unzip 3_c.jpg
Archive:  3_c.jpg
warning [3_c.jpg]:  123606 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: base_images/4_c.jpg

$ cd base_images/ && ls
4_c.jpg

$ strings 4_c.jpg | tail
)PX15
w2qQ
EgubA
equiw
sBUO]
IEND
flag.txtUT
O`ux
flag.txtUT
O`ux

$ unzip 4_c.jpg
Archive:  4_c.jpg
warning [4_c.jpg]:  79578 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: flag.txt

$ cat flag.txt
picoCTF{336cf6d51c9d9774fd37196c1d7320ff}
```

## Wireshark doo dooo do doo...
Flag: `picoCTF{p33kab00_1_s33_u_deadbeef}`

Can you find the flag? shark1.pcapng.

Inspecting the packets, I noticed most of the HTTP packets are encrypted but one isn't, so I extracted the text in the HTML. After extracting, I noticed that the text are encrypted using caesar cipher.

![3](/assets/posts/picoctf/forensics/3.png)

```
$ echo 'Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}' | tr 'a-zA-Z' 'n-za-mN-ZA-M'
The flag is picoCTF{p33kab00_1_s33_u_deadbeef}
```

## Wireshark twoo twooo two twoo...
Flag: `picoCTF{dns_3xf1l_ftw_deadbeef}`

Can you find the flag?

Inspecting the pcap file, I noticed a lot of http get requests requesting for **/flag**. Following the http streams gave random flags. After trying a few and failing, I noticed a strange dns query for the domain **reddshrimpandherring**. Filtering all dns queries I noticed that all of them have strange subdomains that also resembles base64. I also noticed that some of all of the dns comes from two ip source. First from **8.8.8.8** which is Google's dns server and the other unfamiliar ip which is **18.217.1.57**. So I tried extracting all the subdomains from the suspicious IP using tshark and parsed the data using gnu tools before decoding it using base64.

```
$ tshark -r shark2.pcapng -Y "dns.flags.response == 1" -Y "ip.src == 18.217.1.57" -T fields -e dns.qry.name | grep -v ^$ | cut -d'.' -f1 | uniq | tr -d '\n' | base64 -d && echo
picoCTF{dns_3xf1l_ftw_deadbeef}
```

## Disk, disk, sleuth!
Flag: ``

Use `srch_strings` from the sleuthkit and some terminal-fu to find a flag in this disk image: dds1-alpine.flag.img.gz


## tunn3l v1s10n
Flag: ``

We found this file. Recover the flag.


## Trivial Flag Transfer Protocol
Flag: ``

Figure out how they moved the flag.


## What Lies Within
Flag: `picoCTF{h1d1ng_1n_th3_b1t5}`

There's something in the building. Can you retrieve the flag?

```
$ zsteg buildings.png | grep -ioE picoctf{.+}
picoCTF{h1d1ng_1n_th3_b1t5}
```

## extensions
Flag: `picoCTF{now_you_know_about_extensions}`

This is a really weird text file TXT? Can you find the flag?

Upon opening the text file, I noticed that it contains NULL bytes so it must be a binary file. So I ran the file command on the file and found out that it's actually an image file. Saying that, I copied the file into a png file and got the flag.

```
$ file flag.txt
flag.txt: PNG image data, 1697 x 608, 8-bit/color RGB, non-interlaced

$ cp flag.txt flag.png
```

![4](/assets/posts/picoctf/forensics/4.png)


## Glory of the Garden
Flag: `picoCTF{more_than_m33ts_the_3y3657BaB2C}`

This garden contains more than it seems.

Running strings on the image reveals the flag.

```
$ strings garden.jpg | grep -ioE picoctf{.+}
picoCTF{more_than_m33ts_the_3y3657BaB2C}
```

## shark on wire 1
Flag: `picoCTF{StaT31355_636f6e6e}`

We found this packet capture. Recover the flag.

Inspecting the packets in wireshark, I noticed that some packets are 

![5](/assets/posts/picoctf/forensics/5.png)

```
tshark -r capture.pcap -Y"udp.stream==6" -Tfields -edata | tr '\n' ' ' | tr -d ' ' | xxd -p -r && echo
```

## shark on wire 2
Flag: `picoCTF{p1LLf3r3d_data_v1a_st3g0}`

We found this packet capture. Recover the flag that was pilfered from the network.

Ins

## So Meta
Flag: `picoCTF{s0_m3ta_fec06741}`

Find the flag in this picture.

```
$ strings pico_img.png | grep -ioE picoctf{.+}
picoCTF{s0_m3ta_fec06741}
```

## Blast from the past
Flag: ``

The judge for these pictures is a real fan of antiques. Can you age this photo to the specifications?
Set the timestamps on this picture to 1970:01:01 00:00:00.001+00:00 with as much precision as possible for each timestamp. In this example, +00:00 is a timezone adjustment. Any timezone is acceptable as long as the time is equivalent. As an example, this timestamp is acceptable as well: 1969:12:31 19:00:00.001-05:00. For timestamps without a timezone adjustment, put them in GMT time (+00:00). The checker program provides the timestamp needed for each.
Use this picture.

## Mob psycho
Flag: `picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_a3eb5ac2}`

Can you handle APKs?

```
$ find . -name *flag* -exec cat {} \;
7069636f4354467b6178386d433052553676655f4e5838356c346178386d436c5f61336562356163327d
gekko@DESKTOP-45USNEQ:/mnt/d/ctf/pico/Forensics/Mob psycho$ find . -name *flag* -exec cat {} \; | xxd -p -r
picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_a3eb5ac2}
```

## endianness-v2
Flag: `picoCTF{cert!f1Ed_iNd!4n_s0rrY_3nDian_6d3ad08e}`

Here's a file that was recovered from a 32-bits system that organized the bytes a weird way. We're not even sure what type of file it is.

```
To_Hex('Space',0)
Swap_endianness('Hex',4,true)
From_Hex('Auto')
Render_Image('Raw')

```

## Dear Diary
Flag: ``

If you can find the flag on this disk image, we can close the case for good!

## MSB
Flag: `picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_3a219174}`

This image passes LSB statistical analysis, but we can't help but think there must be something to the visual artifacts present in this image...

```
Extract_LSB('R','G','B','','Row',7)
```

## Invisible WORDs
Flag: ``

Do you recognize this cyberpunk baddie? We don't either. AI art generators are all the rage nowadays, which makes it hard to get a reliable known cover image. But we know you'll figure it out. The suspect is believed to be trafficking in classics. That probably won't help crack the stego, but we hope it will give motivation to bring this criminal to justice!

## Torrent Analyze
Flag: ``

SOS, someone is torrenting on our network.
One of your colleagues has been using torrent to download some files on the company’s network. Can you identify the file(s) that were downloaded? The file name will be the flag, like picoCTF{filename}. Captured traffic.

```

```