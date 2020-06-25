---
template: post
title: IPFS vs. HTTP
slug: eighty-second-post
draft: false
date: 2020-02-05T19:30:00.000Z
description: This is a post introducing the InterPlanetary File System and
  briefly how to use it.
category: P2P
tags:
  - Blockchain
  - Decentralized
  - P2P
---

In this post, we will discover briefly go over the HTTP protocol definition, IPFS and it's installation, and how to know how to use IPFS. Let's start with where we are now. 

##HTTP Protocol: Defined

>"HTTP is the protocol that web browsers and web servers use to communicate with each other over the Internet. It is an application level protocol because it sits on top of the TCP layer in the protocol stack and is used by specific applications to talk to one another. In this case the applications are web browsers and web servers." 
- [Shuler 2005](http://www.theshulers.com/whitepapers/internet_whitepaper/index.html)

##IPFS vs. HTTP

![http vs. ipfs](https://www.maxcdn.com/one/media/interplanetary-file-system.png)
Source: [MaxCDN](https://www.maxcdn.com/one/visual-glossary/interplanetary-file-system/)

## Install and Use IPFS

On a Mac OS X, use the following commands. [More install instructions here](https://docs.ipfs.io/introduction/install/)

```
tar xvfz go-ipfs.tar.gz
$ cd go-ipfs
$ ./install.sh
```

Test out IPFS by running `ipfs help` in your command line. If you get an output similar to this: 
```
USAGE: 

ipfs - Global p2p merkle-dag filesystem.
...
```

The installation worked! 

By looking through options listed in `ipfs help`, you can figure out basic commands for using ipfs!

##Conclusion

 Now, you know how to use the basics of IPFS and can get started using the new decentralized, p2p protocol that may well govern the future of the web. 

Sources: 

- [Bostoen, Medium, 2018](https://medium.com/coinmonks/a-hands-on-introduction-to-ipfs-ee65b594937)
- [Case, TechCrunch, 2015](https://techcrunch.com/2015/10/04/why-the-internet-needs-ipfs-before-its-too-late/)
- [Shuler 2005](http://www.theshulers.com/whitepapers/internet_whitepaper/index.html)
- [MaxCDN: IPFS vs. HTTP](https://www.maxcdn.com/one/visual-glossary/interplanetary-file-system/)

Further Information: 
- [IPFS](https://ipfs.io/)
