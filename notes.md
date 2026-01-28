## 2. What is a socket ?

- socket: A way to speak to other programs using standard Unix file descriptors

- In Unix, every I/O by a program is done by reading or writing to a file descriptor. A file descriptor is simply an integer associated with an open file.
- The file could be a ...
  - network connection
  - a FIFO(named pipe)
  - a pipe
  - a terminal
  - a real on-the-disk file
  - or just about anything

- So, the communication of a program with other program, over the internet, is done through a file descriptor.
- To get the file descriptor of a socket, use `socket()` and to use it for communication, use `send()` and `recv()`.
	+ Note that since a socket is also a file descriptor, we can use the `read()` and `write()` calls to communicate through the socket. But the `send()` and `recev()` provides much greater control over the transmission.
- There are many kinds of sockets including DARPA internet address(Internet Sockets), path names on a local node(Unix sockets, does it mean a local file), CCITT X.25 addresses (X.25 Sockets that you can safely ignore, which I don't know what it is) and many others depending on which unix flavor we use.  We are, of course, studying only internet sockets.  

### 2.1 Two Types of Internet Sockets

- There are multiple types of internet sockets. But, we'll only talk about 2 here. The "Stream Sockets" and "Datagram Sockets".
	+ Note that the author also recommends to read about the "raw sockets" as they are also very powerful.
- Stream Sockets or "SOCK_STREAM"
	+ Also called connection oriented socket
		+ Provides reliable(guarantee of packet reception using acknowledgements), error-free and ordered data transmission.
	+ Uses TCP(Transmission Control Protocol) protocol(RFC 793)
	+ telnet, ssh use stream sockets
- Datagram Sockets or "SOCK_DGRAM"
	+ Also called connectionless socket
		* Provides no open connection
		* No guarantee of reliability, ordered transmission. Optional error-checking is present.
	+ Uses UDP(User Datagram Protocol) protocol(RFC 768)
	+ Generally used either when a TCP stack is unavailable or when a few dropped packets are not important.
	+ tftp(trivial ftp), dhcpcd(a DHCP client), multiplayer games, streaming audio, video conferencing etc. are some use cases.
		* Note that applications like tftp, dhcpcd are used to transfer binary applications from one host to another. So, they implement their own protocol on top of UDP to ensure all the packets are received. This can be done by just retransmitting the packet for which an ACK packet is not received within say 5s.
	+ UDP is faster than TCP because it does not have to keep a track of packtes etc.
	
### 2.2 Low Level Nonsense and Network Theory

- OSI network model
	+ Has 7 layers
		* Application
		* Presentation
		* Session
		* Transport
		* Network
		* Data Link
		* Physical
	+ It is a reference model, very general. Actual systems may have only some of the layers and/or the line between two layers can be blurred.
- A layered model more consistent with Unix is:
	+ Application Layer(telnet, ftp etc.)
	+ Host-to-Host Transport Layer(TCP, UDP)
	+ Internet Layer(IP and routing)
	+ Network Access Layer(Ethernet, wifi or whatever)

## 3 IP Addresses, `struct`s, and Data Munging



## TODO

- Beej's guide to network concepts: https://beej.us/guide/bgnet0/html/split/

## Interesting Facts

- The sockets API, though started by the Berkeley folk, has been ported to many many platforms, including Unix, Linux, and even Windows.

## NOTE

- IP(Internet Protocol, RFC 791) generally deals with internet routing and is generally not responsible for data integrity.