# Intro to networking

> A note on words. Machine, Node, Server can all be used fairly interchangeably. I try to use Machine to mean the piece that runs the operating system. This could be a Virtual Machine or physical hardware that is running a Hypervisor or physical hardware running an OS. Node means any item on a network. A machine may represent one or several nodes on a network. Server means a specific program running on a machine that listens for and responds to requests on a network. At times I may mess this up but I will do my best not to.

### What is a network

A network is any collection of computers connected to each other. At the most basic form, computers that are connected to each other form a network. The internet is a collection of networks that are connected together. For this lesson, we will be focusing on the most basic network: a few nodes connected either directly to each other or with a switch (or hub).

> Here is where I would draw an example of a network using a few nodes and a single switch


There have been several different protocols used for machines talking to each other. We will be focusing on what is used currently, TCP/IP. This is the backbone of what every network stack is built on, whether it is using physical wires to connect nodes or wireless to connect them.

### Basic IP and Networking

To be able to communicate, each node on a network is given an address by the network, called an **IP Address** and also supplies its own hardware address. A common address given out by a network is something like this: `192.168.1.5`. A machine may have more than one IP address.

This is for IPv4 which is still most of what a System Administrator will be supporting for the time being. We will talk later about IPv6.

A common hardware address, also called a **MAC Address**, is something like this: `A4-C4-94-8F-D8-33` or `A4:C4:94:8F:D8:33`

These two addresses are used so each node can talk to each other node on the network. This would look something like this:

> Computer one is hosting a Minecraft server. Its network address is 192.168.1.110. A person using computer two is trying to connect to this server on their own computer, which has a network address of 192.168.1.67. For computer two to talk to computer one, the following happens:

> - Computer two sees that it is trying to talk to a local node (we will discuss this shortly).  
- It then sends out a request to every node it is connected to saying "Hey, what is the hardware address of 192.168.1.110?"
- Computer one will then say "Oh hey, *I* have 192.168.1.110 as one of my addresses, my hardware address is AA:BB:CC:DD:EE:FF."
- Computer two then says "Oh, ok, let me package this up for you" then takes all of the info it needs to send to the computer one for log in and puts it into an envelope addressed to 192.168.1.110
- Computer two then puts THAT into an envelope addressed to AA:BB:CC:DD:EE:FF and passes it out to the network, where AA:BB:CC:DD:EE:FF will grab it.

This is somewhat simplified but the basic idea of how computers will talk to each other. When we talk about linked networks, where routing becomes important, we'll talk about more complexities that are added.

### Writing IP Addresses and CIDR Notation

IP addresses are generally written as 4 blocks of numbers between 0 and 255 separated by periods. This makes them easier to remember. However, IP addresses in IPv4 are stored internally as a single 32 bit number which goes from 0 to 4,294,967,295. To prove it, feel free to go to [HTTP://3627735396](HTTP://3627735396).

We do this because no one wants to remember that their local computer is 3232235781 and to connect to the printer, they need to put in 3232235801. It is much easier to remember that their IP is 192.168.1.1 and the printer is 192.168.1.25. (I promise that it is easier eventually).

Internally, through, everything is stored as binary. Each of the normal blocks of 4 in an IP address are 8 binary bits or an **octet**. To understand the next portion, brush up on binary a bit.

192.168.1.1 is stored in binary as 11000000101010000000000100000001.

- 192 is stored as 11000000
- 168 is stored as 10101000
- 1 is stored as 00000001

This is important because when we talk about subnets and CIDR notation, it is based on the 32 bits available to an IP address. There are two sections of each IP address, the part used to describe the network it is on and the part used to describe the individual node.

Often times, especially in home networks, you will see addresses like 192.168.1.1 - 192.168.1.255. This means that the 192.168.1 describes the network, and the final octet is used to describe the individual node. If you were to show this in the binary version, the first 24 bits would be used to describe the network (192.168.1) and the last 8 would describe the node.

|network|host|
|---|---|
|1100 0000.1010 1000.0000 0001.|0000 0001|

There are two ways to define how many bits are reserved for each section of the IP Address. One is by just adding the number of network bits used, like this: `192.168.1.0/24`. This notation, either known as **slash notation** or **CIDR notation** is what most network admins will use to define subnets.

The second way is to make a **binary mask**. This means making a new 32 bit number that has all 1s where the network is described and all 0s where the host is described. The same network as above, described this way would look like this:

`network: 11000000101010000000000100000000`  
`subnet:  11111111111111111111111100000000`

Translating this into decimal would look like this:

`network: 192.168.1.0`  
`subnet:  255.255.255.0`

We haven't talked about what a subnet is, but the basic idea is that it describes which computers are part of the local network vs computers which are part of an outside network. So if one node is trying to talk to another in the same subnet, it will talk to the other node directly, using the hardware address. We will do more with subnets and routing shortly.


#### Exercises!

> Have students take common numbers and convert them to binary and back. Also have them convert to and from hexadecimal.



### Subnets and Broadcasts

So what are subnets? A subnet is the way to define the local network. When a node wants to communicate with another node on the same subnet, they should be able to use MAC addresses because they're directly connected. When a node is trying to reach a node outside of its subnet, it is not directly connected so it must find a route to the other node.

Nodes that are connected to the same subnet can also use broadcasts to send packets to all machines in the local hardware system. There are different kinds of broadcasts, from DHCP and ARP requests, to broadcasts for pushing new operating systems out over the network. Broadcasts are generally limited to the local subnet.

> Show some broadcasts and what they might do, including talking about what would happen if two nodes responded to a broadcast (like with DHCP)

### Switches vs Routers

We have been talking about all local networks or **Local Area Networks** or **LANs** so far. Most times, a LAN is a series of nodes that are connected via a switch. The most useful networks are ones that are connected to other networks. This would allow internal LAN nodes to talk to the **Wide Area Network** or **WAN.** An example of this would be the internet.

For one node to communicate with another across the WAN, they need to be able to route to each other. The most basic way for routes to be set up is for a node to have a gateway set. Once a node has a gateway, every request sent to a node that isn't in the local subnet, the request is sent via the gateway. The internal node assumes that the gateway has routing information for any IP it doesn't have local access to.

How does a gateway know how to get to a WAN IP? Each node in a network knows which nodes it is directly connected to and where it should look for nodes that it isn't directly connected to. Routers are a node on the network which include an extra piece of information: which nodes own which IP addresses. Gateways are the name for a router node on a network. Networks can be thought of as trees. Each branch knows about all of its leaves and knows which IPs are on its leaves. This allows routers to be able to route requests quickly and easily.

> Draw this concept, as it can be hard to understand. Show how a router that owns the 8.4.0.0/16 network would send packets to a router that owns the 16.16.0.0/16 network. Draw this concept as a tree and very basic as if there is only a single path that works. Multiple paths aren't needed quite yet.


This is also a simplification, because networks are both trees and circles, so requests don't always have to go up to the root node before going down a new branch. Depending on how the networks are set up, requests from one node to another can take one of many routes, or even have the individual packets of a requests split into multiple routes.

**BGP** is what rules the more circular types of routing, although that goes beyond the scope of what we're talking about currently. We'll get to it later. For now, we can treat our local networks and the networks they're directly connected to as trees for the purpose of working and troubleshooting.


### How do machines communicate?

As I mentioned before, nodes communicate with each other using IPs and MAC addresses. To understand how they talk when going across the WAN we will use the OSI model.

> Draw/Explain the first 4 layers of the OSI model and how two machines on a local network then two machines across the WAN would send packets to each other. A good way of explain is using the envelope analogy.


### How do machines use IPs and Ports

Once a packet makes it to the correct machine via the IP, the machine will then look at the requested port and see if there is a program listening on the given port.

What is a port? It is a number from 0 to 65536 and it is used so a machine on a network can route different requests to different internal processes, much in the same way IPs are used to route network requests to different machines in a network.

There are several common ports that programs run on, although there is no definition of what MUST run on any given port. A website can run on port 3 as easily as it can run on 3000. The only restriction is that two services cannot run on the same port on the same IP. What defines the defaults are the conventions of the programs and protocols.

#### Standard Ports

| Port| Program|
|---|---|
| 80 | HTTP/Web |
| 443 | HTTPS/Secure Web |
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 3389 | Remote Desktop |

By default, Chrome or any web browser will request websites from port 80 or 443. You can override the default but it is there so there are fewer things for people to remember when typing in server addresses.

Internally, a port a number that a program has reserved for itself to listen on. When a packet comes to a machine (via the MAC) and the machine opens it, it then looks at the IP. If the IP matches one of the IPs on the machine (and we're using TCP, assume we are), then the machine looks at the port to see which program should get the packet. From there, the program will continue to unpack the packet until it gets the info that it needs.

### How can multiple server programs run on a single machines

Because a machine can have multiple IPs and each IP has a set of over 65000 ports available, multiple server programs can run on a single machine. This can be done by moving the port, hosting on other IPs, or even putting some type of proxy in place that can look at incoming requests and route them to the correct program based on the info in the request.

> Have students try to set up at least 2 different ways of hosting multiple servers on a machine. Minecraft is a pretty good server to use, since it is all Java based and has both IP and Port settings available in the server.properties file.


### Common Network Services

Most networks aren't set up fully manually, as we've been doing. There are several basic network services that most networks run. The two most basic are **DNS** and **DHCP**.

#### DHCP

**Dynamic Host Configuration Protocol** or **DHCP** is what networks use to automatically configure machines so they have network access. It normally will give out IP Addresses, Subnet information, gateway information and DNS settings. It can also be used to send more information as needed, such at network time info, IP telephony information or more.

DHCP works by listening for broadcast packets on a subnet. When a DHCP server hears a broadcast packet for DHCPDISCOVERY it will offer an address to the machine. The machine then verifies the address with the DHCP server directly and if approved, the machine will then use the DHCP info given to it.

#### DNS

**Domain Name System** (ain't nobody use that name) or **DNS** is a network service that translates server names into IP addresses. This is also referred to as Name Resolution or DNS Resolver.

Without a DNS server, everyone would have to remember every IP of every website they want to go to. This is not very useful, obviously.

DNS is a much larger topic in the future. DNS is the backbone to almost every aspect of Microsoft Active Directory and is generally the first service to check when there is a network outage. We will dive more deeply into DNS in week two.
