Peer-to-Peer Network Client
===========================

Overview
--------
Our peer-to-peer network consists of two kinds of nodes: trackers and peers. Trackers keep track of which peers are connected to the network; peers actually download files from each other. Peer-to-peer communication in the OSP2P system is built from a series of remote procedure calls (RPCs).

Parallel Uploads and Downloads
------------------------------
My program is implemented using the forking method. We fork once for every download request, then wait for all the child processes to finish. After that we fork once for every upload request and continuously check to see if the child processes have finished.

Buffer Overflow
---------------
The biggest vulnerability is this program was the use of strcpy. I switched every instance to strncpy and used FILENAMESIZ as the hard length. I also checked to see if the filename was ever larger than the FILENAMESIZ max.

Other Vulnerabilities
---------------------
* I ensured that the program only uploaded files in the current directory.
* I increased the number of maximum peers so the program could work with the popular tracker.
* I established a maximum file size so that another peer could not fill up my disk with an extremely large file.

Download Attack
---------------
The first attack requests a file with a filename larger than FILENAMESIZ. During each iteration it increases the filename by one character (just in case somebody tries to block too many transfers of the same thing).

This is a buffer overflow attack which in it's current state will only crash the user's program, but can be injected with mallicious code to do all sorts of nasty things.

It is also a denial of service attack because it will continue to loop this request. So even if the client implemented our filename solution, they would have to deal with our repeating requests. This has the possibility to crash the client.

Upload Attack
-------------
The second attack sends an extrememly large file (dev/urandom) which will eventually fill up the peer's disk.

This attack will cause the client to eventually crash because there will be no space on the disk.

Full project specification: http://www.read.cs.ucla.edu/111/lab4
