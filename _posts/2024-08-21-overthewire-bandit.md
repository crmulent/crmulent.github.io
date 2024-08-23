---
title: OverTheWire's Writeup
time: 2024-08-19 12:00:00
categories: [general]
tags: []
image: /assets/posts/overthewire/icon.jpg
---

A writeup for OverTheWire's bandit challenges.

# Bandit Level 0

## Level Goal
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

## Commands you may need to solve this level
ssh

## Helpful Reading Material
[Secure Shell (SSH) on Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)
[How to use SSH on wikiHow](https://www.wikihow.com/Use-SSH)


```
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
```

After running the command above, we were asked if we wanted to continue connecting to the server. We just need to type yes and enter to continue.

```
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([13.50.165.192]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Next we are prompted to enter the password for the user. We can do this by typing the password given to us which is **bandit0**.

```
Warning: Permanently added '[bandit.labs.overthewire.org]:2220' (ED25519) to the list of known hosts.
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password:
```

After that, we were greeted by a banner 