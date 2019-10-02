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
```assembly
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
```

__Day 8__:

I continued my investigation regarding the above program. For that, I started reading the paper _PC Assembly Language_ available at https://zoo.cs.yale.edu/classes/cs422/2014fa/readings/pcasm-book.pdf. I familiared with concepts such as the stack, the CPU registers, etc. which are essential to the low-level functioning of the computer. I decided to study and work with the x86 (32 bit) architecture (specifically the Intel CPU 80386) for its the most common one when it comes to starting with assembly/reverse engineering. For the hands-on lab I decided to write the simplest program one can think of in the C language:
```c
int main() {
	return 0;
}
```

I compiled it with `gcc -g init.c`. Said flag includes in the binary extra debugging information, which will aid in the static analysis of the binary. To see the machine code that said script produced I ran the command `objdump -M intel -D init | grep -A 5 main\>`. The output of said command which disassembles the `init` binary and uses Intel syntax (instead of the default AT&T) is the following:
```assembly
080483ed <main>:
 80483ed:	55                   	push   ebp
 80483ee:	89 e5                	mov    ebp,esp
 80483f0:	b8 00 00 00 00       	mov    eax,0x0
 80483f5:	5d                   	pop    ebp
 80483f6:	c3                   	ret
```

The above output is the machine code (the ones and zeros the bare 80386 CPU understands). The first column represents the memory addresses —in hexadecimal format— of each of the different instructions (there is one per line). As we can see, the first memory address is `0x80483ed`; the instruction `0x55` which corresponds to the mnemonic `push ebp` is located there. As we can tell by its opcode (`0x55`), this instruction is 1 byte (8 bits) long. Thus, the next instruction begins 1 byte after it, at `0x80483ee`, and so on and so forth. The guide by the University of Virginia Computer Science department found at https://www.cs.virginia.edu/~evans/cs216/guides/x86.html helped me understand the C calling convention. I focused on the callee rules which are the ones of interest in this case, since we are not calling another subroutine (or function), we are not the callers in this case therefore we do not need to worry about those conventions for now. The instruction `mov eax,0x0` sets the subroutine's return value by moving the byte `0x0` (0 in decimal) to the register `EAX`, which conventionally contains the return code. The second-to-last instruction restores the value of the original pushed `EBP`. Finally, `ret` returns to the caller. This opcode finds and removes the appropriate return address from the stack.

After doing researching for a couple hours, the program described in day 6 finally made sense. The critical instructions are the following (the rest just follows the callee rules):
```assembly
	mov	eax,DWORD PTR [ebp+0x8]
	mov	ebx,DWORD PTR [ebp+0xc]
	mov	eax,ebx
```

The first line copies the value residing at `EBP` + 1 byte to EAX. Said value is `0xc9`, since it is the first argument (normally located at `EBP+0x8`) of the function called. Next, the second argument, found at `EBP+0x8` which equals `EBP+0x8+0x4` (that is, the first argument plus 4 bytes) copies `0xb0` to EBX. Afterwards EBX is moved to EAX, so the return code is `0xb0`!


__Day 9__:

Since today's class was about relational databases and I had some experience on that field (mainly MySQL and MariaDB), I decided to use my class time for working on my CS EE. I have learnt about Man-in-the-middle (MITM) attacks, which consists of a third party intercepting the network connection between a client and a server. In the context of a LAN, this is usually performed via ARP spoofing, which is a technique that involves sending maliciously crafted packets to the network so that the switch/router in place sends the packets destined to the legitimate client to the third party.

After that I continued my research and explanation of the Tor network, focusing on its characteristics of P2P (decentralised) network and its robust cryptographic scheme of 3 layers (hence the name, The __Onion__ Router).

__Day 10__:

Today I decided to investigate about accomplishing SQL injection even when `mysql[i?]_real_escape_string` is santising the input. After reading the documentation in php.net I did not find anything relevant to security concerns except that the characters '%' and '\_' are not parsed, ie escaped. So I decided to google about if SQLi is possible with the given statement. I came up to [this](https://stackoverflow.com/questions/5741187/sql-injection-that-gets-around-mysql-real-escape-string/12118602#12118602) detailed thread comment which claims that "there is a way to get around mysql_real_escape_string()". The attack or injection is the following:

```php
mysql_query('SET NAMES gbk');
$var = mysql_real_escape_string("\xbf\x27 OR 1=1 /*");
mysql_query("SELECT * FROM test WHERE name = '$var' LIMIT 1");
```

The first line is critical to the following exploitation, for it sets a special character set (gbk) on the server which will eventually decode the hexadecimal string 0x27 which refers to the ASCII literal `'`. This character is what triggers the injection since it will close the single quote opened for `$var` and therefore will insert a trivial SQL statement. Because of the nature of the configured character set the executed statement results to: `縗' OR 1=1 /*`

As we have seen, this attack requires us to manually set a specific character enconding on the backend server. Since this is not possible given the provided PHP code (that is, we do not have the ability to control such parameter), a SQL injection is not possible in this code fragment.

The comment author concludes saying that to be safe we should not use a vulnerable character set like the one described previously. UTF8, ascii, and latin1 are safe options and normally the default of many operating systems and frameworks. Either way, the recommended way to prevent SQL injection by the developer and infosec communities is to use PHP `prepare` statements like this:
```php
$stmt = $pdo->prepare('SELECT * FROM table WHERE YEAR(created) = ? AND MONTH(created) = ?');
if ($stmt->execute([$_GET['year'], $_GET['month']])) {
    $posts = $stmt->fetchAll(\PDO::FETCH_ASSOC);
}
```

Code sample taken from [here](https://paragonie.com/blog/2015/05/preventing-sql-injection-in-php-applications-easy-and-definitive-guide).

__Day 11__:

We learnt about recursion and we were supossed to write a program related with series analysis with said technique. Specifically, we had to find the mean of x series of elements in an array. The x was labeled level. I came up with an iterative, faster solution by using a formula to break up the whole array into the desired number of chunks. Here it is:

```python
ARR    = [4, 5, 8, 29, 85, 4, 2, 90]
LEVELS = 1
RESULT = []

offset = int(len(ARR) / 2**LEVELS)
chunks = range(0, len(ARR), offset)

print('Offset:', offset)

for chunk in chunks:
    print('Chunk:', chunk)
    numbers = ARR[chunk:chunk+offset]
    RESULT.append(sum(numbers) / len(numbers))

print(RESULT)
```
