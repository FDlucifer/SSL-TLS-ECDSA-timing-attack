# Timing/Lattice Attack on the ECDSA (binary curves) nonces of OpenSSL

This is a work trying to reproduce and improve on *Billy Bob Brumley* and *Nicola Tuveri* - [Remote Timing Attacks are Still Practical](https://eprint.iacr.org/2011/232.pdf).

You can reproduce my setup with what you find here. The lattice attack works. The remote timing is not precise enough to make the attack work. If you can get the same setup and better timing that what I get below then you should contact me :)

It works on an unpatched version of OpenSSL, but theorically it should work on any TLS framework that has such a timing attack (and not only on binary curves).

But first, if you want to know more about this research [check the latest draft of the whitepaper](whitepaper.pdf), and here are also direct links to the [Timing Attack](setup/client/attack.c) and the [Lattice Attack](setup/client/offline/lattice.sage). And also a [demo](https://www.youtube.com/watch?v=P2NbKHn7RkI&feature=youtu.be) of the attack.

If you know more about how to collect extremely accurate timing samples on a remote target I might need you. These are the last results I get from a million signatures:

![stats](http://i.imgur.com/Lt2Z5gD.png)

## Structure

* in `setup/` you can find how to setup the server and the client to reproduce the attack (and how to modify the server's openSSL to remove the fix)

* in `datasets/` you have data I got from my own experiments. You can play with that if you don't want to setup a client/server. Note that my measurements from the client sucks

* in `tools/` you have tools to play with the data in `datasets/`. Read the README there for more info.

* `PoC/` is an old proof of concept, it can run and find a key. It's not very pretty though


## To Do/Try

* Time with `SO_TIMESTAMPING` on raw sockets. Use a NIC that allows for hardware TCP timestamping.
* Find other ways to optimize the network card ([Tuning 10Gb network cards on Linux by Breno Henrique Leitao, IBM](https://wiki.chipp.ch/twiki/pub/CmsTier3/NodeTypeFileServerHPDL380G7/ols2009-pages-169-1842.pdf)).
* Time UDP packet instead (and target DTLS).
* Look at Nguyen way's of attacking ECDSA, he seems to build his lattice differently.
* Modify the ClientHello from the timing attack to only accept ECDHE-ECDSA... (so that we can test it against different frameworks)
* Truncate the hash correctly in the timing attack
