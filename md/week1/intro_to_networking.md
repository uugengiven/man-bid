# Intro to networking

### What is a network

A network is as basic as two computers connected together. It can be as complex as the entire internet, which is a huge collection of computers all connected to each other.

At the most basic form, computers that are connected to each other form a network. The internet is a collection of networks that are connected together. For this lesson, we will be focusing on networks that are simple: a few nodes connected either directly to each other or with a switch (or hub).

-- Here is where I would draw an example of a network using a few nodes and a single switch --

There have been several different protocols used for machines talking to each other. We will be focusing on what is used currently, TCP/IP. This is the backbone of what every network stack is built on, whether it is using physical wires to connect nodes or wireless to connect them.

### Basic IP and Routing

To be able to communicate, each node on a network is given an address by the network, called an **IP Address** and also supplies its own hardware address. A common address given out by a network is something like this: `192.168.1.5`

This is for IPv4 which is still most of what a System Administrator will be supporting for the time being. We will talk later about IPv6.

A common hardware address, also called a **MAC Address**, is something like this: `A4-C4-94-8F-D8-33` or `A4:C4:94:8F:D8:33`

These two addresses are used so each node can talk to each other node on the network. This would look something like this:

> Computer one is hosting a Minecraft server. It's network address is 192.168.1.110. A person using computer two is trying to connect to this server on their own computer, which has a network address of 192.168.1.67. For computer two to talk to computer one, the following happens:

> - Computer two sees that it is trying to talk to a local node (we will discuss this shortly).  
- It then sends out a request to every node it is connected to saying "Hey, what is the hardware address of 192.168.1.110?"
- Computer one will then say "Oh hey, *I* am 192.168.1.110, my hardware address is AA:BB:CC:DD:EE:FF."
- Computer two then says "Oh, ok, let me package this up for you" then takes all of the info it needs to send to the computer one for log in and puts it into an envelope addressed to 192.168.1.110
- Computer two then puts THAT into an envelope addressed to AA:BB:CC:DD:EE:FF and passes it out to the network, where AA:BB:CC:DD:EE:FF will grab it.

This is somewhat simplified but the basic idea of how computers will talk to each other. When we talk about linked networks, where routing becomes important, we'll talk about more complexities that are added.

### CIDR Notation

IP addresses are generally written as 4 blocks of numbers between 0 and 255 separated by periods. This makes them easier to remember. However, IP addresses in IPv4 are stored internally as a single 32 bit number which goes from 0 to 4,294,967,295. To prove it, feel free to go to [HTTP://3627735396](HTTP://3627735396).

We do this because no one wants to remember that their local computer is 3232235781 and to connect to the printer, they need to put in 3232235801. It is much easier to remember that their IP is 192.168.1.1 and the printer is 192.168.1.25.

Internally, through, everything is stored as binary. Each of the normal blocks of 4 in an IP address are 8 binary bits or an **octet**. To understand the next portion, brush up on binary a bit.

192.168.1.1 is stored in binary as 11000000101010000000000100000001.

- 192 is stored as 11000000
- 168 is stored as 10101000
- 1 is stored as 00000001

This is important because when we talk about subnets and CIDR notation, it is based on the 32 bits available to an IP address. There are two sections of each IP address, the part used to describe the network it is no and the part used to describe the individual node.

Often times, especially in home networks, you will see address like 192.168.1.1 - 192.168.1.255. This means that the 192.168.1 describes the network, and then final octet is used to describe the individual node. If you were to show this on the binary version, the first 24 bits would be used to describe the network (192.168.1) and the last 8 would describe the node.

|network|host|
|---|---|
|1100 0000.1010 1000.0000 0001.|0000 0001|

There are two ways to define how many bits are reserved for each section of the IP Address. One is by just adding the number of network bits used, like this: `192.168.1.0/24`

The second way is to make a **binary mask**. This means making a new 32 bit number that has all 1s where the network is described and all 0s where the host is described. The same network as above, described this way would look like this:

`network: 11000000101010000000000100000000`  
`subnet:  11111111111111111111111100000000`

Translating this into decimal would look like this:

`network: 192.168.1.0`  
`subnet:  255.255.255.0`

We haven't talked about what a subnet is, but the basic idea is that it describes which computers are part of the local network vs computers which are part of an outside network. So if one node is trying to talk to another in the same subnet, it will talk to the other node directly, using the hardware address. We will do more with subnets and routing shortly.

```
Exercises!

Have students take common numbers and convert them to binary and back. Also have them convert to and from hexadecimal.
```


### Subnets and Broadcasts

### Switches vs Routers

### How do machines talk?

### How do machines use IPs and Ports

### How can multiple server programs run on a single machines
