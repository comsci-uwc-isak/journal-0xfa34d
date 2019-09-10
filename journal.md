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
