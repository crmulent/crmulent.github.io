---
title: TryHackMe (TShark) - Writeup
time: 2024-05-06 12:00:00
categories: [tryhackme, forensics]
tags: [tshark, linux]
image: /assets/posts/tryhackme/icon.png
---

A writeup for TryHackMe's Tshark room.

## Reading PCAP Files
For this task, we were given a PCAP file called **dns_1617454996618.cap**

##### Question: How many packets are in the dns.cap file?

Flag: `38`

To analyze the PCAP file, we can use the **-r** option of tshark. This allows us to see all the packets of the PCAP file. To get the flag, simply look at the last packet's index.

```
$ tshark -r dns_1617454996618.cap | tail
   29 271.262407 192.168.170.20 → 192.168.170.8 DNS 166 Standard query response 0x208a NS isc.org NS ns-ext.nrt1.isc.org NS ns-ext.sth1.isc.org NS ns-ext.isc.org NS ns-ext.lga1.isc.org
   30 271.279695  217.13.4.24 → 192.168.170.56 DNS 129 Standard query response 0x326e No such name SRV _ldap._tcp.Default-First-Site-Name._sites.dc._msdcs.utelsystems.local
   31 271.280350 192.168.170.56 → 217.13.4.24  DNS 98 Standard query 0xf161 SRV _ldap._tcp.dc._msdcs.utelsystems.local
   32 271.297651  217.13.4.24 → 192.168.170.56 DNS 98 Standard query response 0xf161 No such name SRV _ldap._tcp.dc._msdcs.utelsystems.local
   33 271.298194 192.168.170.56 → 217.13.4.24  DNS 140 Standard query 0x8361 SRV _ldap._tcp.05b5292b-34b8-4fb7-85a3-8beef5fd2069.domains._msdcs.utelsystems.local
   34 271.317878  217.13.4.24 → 192.168.170.56 DNS 140 Standard query response 0x8361 No such name SRV _ldap._tcp.05b5292b-34b8-4fb7-85a3-8beef5fd2069.domains._msdcs.utelsystems.local
   35 271.419659 192.168.170.56 → 217.13.4.24  DNS 83 Standard query 0xd060 A GRIMM.utelsystems.local
   36 271.436583  217.13.4.24 → 192.168.170.56 DNS 83 Standard query response 0xd060 No such name A GRIMM.utelsystems.local
   37 278.861300 192.168.170.56 → 217.13.4.24  DNS 83 Standard query 0x7663 A GRIMM.utelsystems.local
   38 278.879313  217.13.4.24 → 192.168.170.56 DNS 83 Standard query response 0x7663 No such name A GRIMM.utelsystems.local
```

##### Question: How many A records are in the capture? (Including responses)

Flag: `6`

To get all the A records, we can set the display filter to **dns.qry.type == 8** using **-Y** option. We can also make things easier by piping the output to the **wc** command to count all the lines. To learn more about DNS types, click [here](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#table-dns-parameters-4)

```
$ tshark -r dns_1617454996618.cap -Y "dns.qry.type == 1"
    9  92.189905 192.168.170.8 → 192.168.170.20 DNS 74 Standard query 0x75c0 A www.netbsd.org
   10  92.238816 192.168.170.20 → 192.168.170.8 DNS 90 Standard query response 0x75c0 A www.netbsd.org A 204.152.190.12
   35 271.419659 192.168.170.56 → 217.13.4.24  DNS 83 Standard query 0xd060 A GRIMM.utelsystems.local
   36 271.436583  217.13.4.24 → 192.168.170.56 DNS 83 Standard query response 0xd060 No such name A GRIMM.utelsystems.local
   37 278.861300 192.168.170.56 → 217.13.4.24  DNS 83 Standard query 0x7663 A GRIMM.utelsystems.local
   38 278.879313  217.13.4.24 → 192.168.170.56 DNS 83 Standard query response 0x7663 No such name A GRIMM.utelsystems.local
$ tshark -r dns_1617454996618.cap -Y "dns.qry.type == 1" | wc -l
6
```

##### Question: Which A record was present the most?

Flag: `GRIMM.utelsystems.local`

To get which record was present the most, we can append **-T fields -e dns.qry.name** to get the names of the DNS. We can also pipe the output to **sort** and **uniq** commands to count each name's occurrences.

```
$ tshark -r dns_1617454996618.cap -Y "dns.qry.type == 1" -T fields -e dns.qry.name
www.netbsd.org
www.netbsd.org
GRIMM.utelsystems.local
GRIMM.utelsystems.local
GRIMM.utelsystems.local
GRIMM.utelsystems.local
$ tshark -r dns_1617454996618.cap -Y "dns.qry.type == 1" -T fields -e dns.qry.name | sort | uniq -c
      4 GRIMM.utelsystems.local
      2 www.netbsd.org
```

## DNS Exfil
For this task, we were given a PCAP file called **dns_exfil_1617459299197.PCAP**

##### Question: How many packets are in this capture?

Flag: `125`

Just like we did before, we can use the **-r** option to read the PCAP file.

```
$ tshark -r dns_exfil_1617459299197.PCAP | tail
  116   7.245048  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A G.m4lwhere.org
  117   7.263699 192.168.1.200 → 192.168.1.8  DNS 90 Standard query response 0xbeef A G.m4lwhere.org A 52.207.163.69
  118   7.373057  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A W.m4lwhere.org
  119   7.391566 192.168.1.200 → 192.168.1.8  DNS 90 Standard query response 0xbeef A W.m4lwhere.org A 52.207.163.69
  120   7.530259  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A I.m4lwhere.org
  121   7.549069 192.168.1.200 → 192.168.1.8  DNS 90 Standard query response 0xbeef A I.m4lwhere.org A 52.207.163.69
  122   7.681131  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A L.m4lwhere.org
  123   7.760756 192.168.1.200 → 192.168.1.8  DNS 90 Standard query response 0xbeef A L.m4lwhere.org A 52.207.163.69
  124   7.810871  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A 5.m4lwhere.org
  125   7.829560 192.168.1.200 → 192.168.1.8  DNS 90 Standard query response 0xbeef A 5.m4lwhere.org A 52.207.163.69
```

##### Question: How many DNS queries are in this PCAP? (Not responses!)

Flag: `56`

Before we search how many queries there are, first we need to find the right filter. After searching the web, I found out that the way to do this is by using the **-G** option of the tshark. This will cause tshark to dump all available fields. We can also filter the output by piping it to **grep**.

```
$ tshark -G | grep dns | grep response | head
F       Length dns.length     FT_UINT16 dns     BASE_DEC    0x0 Length of DNS-over-TCP request or response
F       Response dns.flags.response     FT_BOOLEAN   dns    16 0x8000   Is the message a response?
F       Conflict dns.flags.conflict     FT_BOOLEAN   dns    16 0x400    Did we receive multiple responses to a query?
F       Response In     dns.response_in FT_FRAMENUM  dns     0x0        The response to this DNS query is in this frame
F       Request In      dns.response_to FT_FRAMENUM  dns     0x0        This is a response to the DNS query in this frame
F       Retransmitted response. Original response in dns.retransmit_response_in FT_FRAMENUM     dns             0x0     This is a retransmitted DNS response
F       Unsolicited     dns.unsolicited FT_BOOLEAN   dns    0 0x0       This is an unsolicited response
F       DNS response retransmission     dns.retransmit_response FT_NONE dns             0x0
F       DNS response(s) cflow.pie.ntop.dns_response  FT_STRING cflow            0x0
F       DnsResponseCode cflow.pie.gigamon.dnsresponsecode   FT_UINT8    cflow   BASE_DEC        0x0
```

Now we can use the **dns.flags.response** as a display filter.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | head
    1   0.000000  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A M.m4lwhere.org
    4   0.137203  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A Z.m4lwhere.org
    7   0.278367  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A W.m4lwhere.org
   10   0.411408  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A G.m4lwhere.org
   13   0.546370  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A C.m4lwhere.org
   16   0.685319  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A Z.m4lwhere.org
   19   0.836122  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A 3.m4lwhere.org
   20   0.961352  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A 3.m4lwhere.org
   24   1.105976  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A O.m4lwhere.org
   26   1.263586  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A R.m4lwhere.org
```

After verifying that we have the right output. We can now count the packets.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | wc -l
56
```

##### Question: What is the DNS transaction ID of the suspicious queries (in hex)?

Flag: `0xbeef`

Doing what we did above, we can find the appropriate field for this.

```
$ tshark -G | grep dns | grep transaction
F       Transaction ID  dns.id  FT_UINT16       dns  BASE_HEX 0x0       Identification of transaction
F       DNS query transaction Id        cflow.pie.ntop.dns_query_id     FT_UINT16       cflow   BASE_DEC        0x0
F       DNS Transaction Id    cflow.pie.ixia.dns-transaction-id FT_UINT16       cflow   BASE_HEX        0x0     DNS Transaction Identifier
```

We can then use this field to get the flag

```
$ tshark -r dns_exfil_1617459299197.PCAP -T fields -e dns.id | uniq
0xbeef
```

##### Question: What is the string extracted from the DNS queries?

Flag: `MZWGCZ33ORUDC427NFZV65BQOVTWQX3XNF2GQMDVG5PXI43IGRZGWIL5`

Remember how we used to filter all the DNS queries? We need to do it again to get the flag. Notice that after filtering all the DNS queries, we can see that each entry has a different subdomain.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | head
    1   0.000000  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A M.m4lwhere.org
    4   0.137203  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A Z.m4lwhere.org
    7   0.278367  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A W.m4lwhere.org
   10   0.411408  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A G.m4lwhere.org
   13   0.546370  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A C.m4lwhere.org
   16   0.685319  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A Z.m4lwhere.org
   19   0.836122  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A 3.m4lwhere.org
   20   0.961352  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A 3.m4lwhere.org
   24   1.105976  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A O.m4lwhere.org
   26   1.263586  192.168.1.8 → 192.168.1.200 DNS 74 Standard query 0xbeef A R.m4lwhere.org
```

This is my favorite part! Using the standard GNU tools, we can manipulate the output to get the flag.

**`rev`** - Reverses each line of text.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | rev | head
gro.erehwl4m.M A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  000000.0   1
gro.erehwl4m.Z A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  302731.0   4
gro.erehwl4m.W A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  763872.0   7
gro.erehwl4m.G A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  804114.0   01
gro.erehwl4m.C A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  073645.0   31
gro.erehwl4m.Z A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  913586.0   61
gro.erehwl4m.3 A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  221638.0   91
gro.erehwl4m.3 A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  253169.0   02
gro.erehwl4m.O A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  679501.1   42
gro.erehwl4m.R A feebx0 yreuq dradnatS 47 SND 002.1.861.291 → 8.1.861.291  685362.1   62
```

**`cut -d'.' -f3`** - Splits the string with a dot as a delimiter then gets the third string in the array.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | rev | cut -d'.' -f3 | head
M A feebx0 yreuq dradnatS 47 SND 002
Z A feebx0 yreuq dradnatS 47 SND 002
W A feebx0 yreuq dradnatS 47 SND 002
G A feebx0 yreuq dradnatS 47 SND 002
C A feebx0 yreuq dradnatS 47 SND 002
Z A feebx0 yreuq dradnatS 47 SND 002
3 A feebx0 yreuq dradnatS 47 SND 002
3 A feebx0 yreuq dradnatS 47 SND 002
O A feebx0 yreuq dradnatS 47 SND 002
R A feebx0 yreuq dradnatS 47 SND 002
```

**`cut -d' ' -f1`** - Splits the string with space as a delimiter then gets the first string in the array.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | rev | cut -d'.' -f3 | cut -d' ' -f1 | head
M
Z
W
G
C
Z
3
3
O
R
```

**`tr -d '\n' && echo`** - Removes newline characters from the output and prints a newline.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | rev | cut -d'.' -f3 | cut -d' ' -f1 | tr -d '\n' && echo
MZWGCZ33ORUDC427NFZV65BQOVTWQX3XNF2GQMDVG5PXI43IGRZGWIL5
```

##### Question: What is the flag?

Flag: `flag{th1s_is_t0ugh_with0u7_tsh4rk!}`

With my previous experiences playing CTF, I know that the string we got from the previous question is encoded using base32 because it only uses capital letters. We can verify this by piping **base32 -d** to the previous command, before echoing.

```
$ tshark -r dns_exfil_1617459299197.PCAP -Y "dns.flags.response == 0" | rev | cut -d'.' -f3 | cut -d' ' -f1 | tr -d '\n' | base32 -d && echo
flag{th1s_is_t0ugh_with0u7_tsh4rk!}
```

Or we can just use the **magic** operation in **Cyberchef**
![1](/assets/posts/tryhackme/tshark/1.png)