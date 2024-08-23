---
title: Root-me (Network) - Writeup
time: 2024-08-19 12:00:00
categories: [network]
tags: []
image: /assets/posts/root-me/icon.svg
---

A writeup for Root-me's network challenges.

# Network

## FTP - authentication
Flag: `cdts3500`

Packet capture analysis

An authenticated file exchange achieved through FTP. Recover the password used by the user.

Opening the pcap file using tshark, we can see that there are different protocols. We are only interested in the FTP protcol since it is indicated in the statement that we need to find the password. To speed up the process, I used tshark to read the file and grepped the password.

```
$ tshark -r ch1.pcap -Y ftp | grep -i pass
    9   4.217350 10.20.144.151 → 10.20.144.150 FTP 91 Response: 331 Enter password.
   11   7.639420 10.20.144.150 → 10.20.144.151 FTP 81 Request: PASS cdts3500
   29  20.787770 10.20.144.151 → 10.20.144.150 FTP 121 Response: 227 Entering Passive Mode (10,20,144,151,62,141).
```
