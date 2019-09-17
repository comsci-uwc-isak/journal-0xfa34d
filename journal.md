__Day 1__:
- tracing algorithms
- logic gates
- blockhain
- ROT13
- basic \*NIX terminal knowledge
- ASL (American Sign Language)

__Day 2__:

We had the task to make a bash script to calculate the value of `E`. We had to use conditionals, loops, etc. Here is my solution to the problem:

![404 not found](/day_2.png)

__Day 3__:

We went over basic network topologies by size and connections. After reading an article about this we did an exercise on the whiteboard to †est our knowledge.

For practising what we just learnt we connected to the Computer Science class raspberry pies via ssh like this:
`ssh pi@192.168.4.150`

We ssh'ed from that machine to another one with the same command. Also, we practised more bash scripting by creating a file with our name with CLI text editor `nano`.

__Day 4__:

We learnt about the OSI model and its 7 layers (Application, Representation, Session, Transport, Network, Data Link, and Physical). This knowledge helps us understand how our data travels through different networks, whether it be our LAN or the Internet itself. We then continued with a lab by sshing ourselves to a raspberry pi. We setup apache by doing `sudo apt-get install apache2`. Then we created an HTML file in the `/var/www/html/` folder with our name and we displayed from the browser by typing the rasperry's IP slash our file name. We then used PugHtml to write our HTML templates faster than before.

__Day 5__:

We setup a VPN (not to be confused with a VNC, which is used for remote desktop!) to connect securely to our Raspberry Pi. We followed the tutorial at http://www.pivpn.io to install OpenVPN with the command `curl -L https://install.pivpn.io | bash
`. Then we created an OpenVPN (.ovpn) profile with the command `pivpn add`. We tried to connect with the VPN client TunnelBlick although it did not work since there was an error with the TLS handshake. Now we are going to try using an RSA key pair instead of ECC, which is less supported due to being recently developed and adopted in the criptographic community.

__Day 6__:

We reconfigured our previous OpenVPN installation using an RSA of 1024 bit (we did not use a larger size key since the CPU power of the raspberry pi is limited, hence it would have taken a lot of time to generate the prime numbers for the key pair). The error was similar, although this time, according to Tunnelblick's —a Mac VPN client— log the server was constantly resetting the connection. We reinstalled the VPN server again by using TCP, since we used UDP all the time, this may be a networking/firewall issue. Again, the error returned was the same: we still could not connect. For the rest of the class I worked on a couple of challenges @ picoCTF 2018.

__Day 7__:

Since I had a bit of experience working with symmetric key cryptography, I decided to continue with my learning of assembly language for the purpose of reverse engineering. I learnt about the general-purpose registers such as EAX and EBX and the basic ones: EBP (base pointer) and ESP (stack pointer). I used `nasm` to assemble a simple script which output text to the console and `ld` to link the generated object file and create an executable. Currently I am trying to answer the question _What does asm0(0xc9,0xb0) return?_ regarding the following program:
`assembly
.intel_syntax noprefix
.bits 32

.global asm0

asm0:
	push	ebp
	mov	ebp,esp
	mov	eax,DWORD PTR [ebp+0x8]
	mov	ebx,DWORD PTR [ebp+0xc]
	mov	eax,ebx
	mov	esp,ebp
	pop	ebp
	ret
`
