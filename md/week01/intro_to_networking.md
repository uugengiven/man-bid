# Intro to networking

> A note on words. Machine, Node, Server can all be used fairly interchangeably. I try to use Machine to mean the piece that runs the operating system. This could be a Virtual Machine or physical hardware that is running a Hypervisor or physical hardware running an OS. Node means any item on a network. A machine may represent one or several nodes on a network. Server means a specific program running on a machine that listens for and responds to requests on a network. At times I may mess this up, but I will do my best not to.

### What is a network

A network is any collection of computers connected to each other. At the most basic form, computers that are connected to each other form a network. The Internet is a collection of networks that are connected together. For this lesson, we will be focusing on the most basic network: a few nodes connected either directly to each other or with a switch (or hub).

> Here is where I would draw an example of a network using a few nodes and a single switch


There have been several different protocols used for machines talking to each other. We will be focusing on what is used currently, TCP/IP. This is the backbone of what every network stack is built on, whether it is using physical wires to connect nodes or wireless signals to connect them.

### Basic IP and Networking

To be able to communicate, each node on a network is given an address by the network, called an **IP Address** and also supplies its own hardware address. A common address given out by a network is something like this: `192.168.1.5`. A machine may have more than one IP address.

This is for IPv4 which is still most of what a System Administrator will be supporting for the time being. We will talk later about IPv6.

A common hardware address, also called a **MAC** (Media Access Control) **Address**, is something like this: `A4-C4-94-8F-D8-33` or `A4:C4:94:8F:D8:33`.  A MAC Address is really more of an identifier than an address.  It provides unique identification of a node's interface, but it doesn't tell where the node is.

These two addresses are used so each node can talk to each other node on the network. This would look something like this:

> Computer one is hosting a Minecraft server. Its network address is 192.168.1.110. A person using computer two is trying to connect to this server on their own computer, which has a network address of 192.168.1.67. For computer two to talk to computer one, the following happens:

> - Computer two sees that it is trying to talk to a local node (that is, they both share a network. We will discuss this shortly).  
- It then sends out a request to every node it is connected to saying "Hey, what is the hardware address of 192.168.1.110?"
- Computer one will then say "Oh hey, *I* have 192.168.1.110 as one of my addresses, my hardware address is AA:BB:CC:DD:EE:FF."
- Computer two then says "Oh, OK, let me package this up for you," then takes all of the info it needs to send to the computer one for log in and puts it into an envelope addressed to 192.168.1.110
- Computer two then puts *that* into an envelope addressed to AA:BB:CC:DD:EE:FF and passes it out to the network, where AA:BB:CC:DD:EE:FF will grab it.

This is somewhat simplified but the basic idea of how computers will talk to each other. When we talk about linked networks, where routing becomes important, we'll talk about more complexities that are added.

### Writing IP Addresses and CIDR Notation

IP addresses are generally written as four numbers between 0 and 255 separated by periods. This makes them easier to remember. However, IP addresses in IPv4 are stored internally as a single 32 bit number which goes from 0 to 4,294,967,295. To prove it, feel free to go to [http://3627735396](http://3627735396) in Firefox, Safari, or Internet Explorer, but not Edge.

We do this because no one wants to remember that their local computer is 3232235781 and to connect to the printer, they need to put in 3232235801. It is much easier to remember that their IP address is 192.168.1.1 and the printer is 192.168.1.25. (I promise that it is easier eventually).

Internally, through, everything is stored as binary. Each of the four numbers in an IP address is eight bits or an **octet**. To understand the next portion, brush up on binary a bit.

192.168.1.1 is stored in binary as 11000000101010000000000100000001.

- 192 is stored as 11000000
- 168 is stored as 10101000
- 1 is stored as 00000001

This is important because when we talk about subnets and CIDR notation, it is based on the 32 bits available to an IPv4 address. There are two parts to an IP address: the part used to identify the network that the node is on, and the part used to identify the individual node within that network.

Often times, especially in home networks, you will see addresses like 192.168.1.1 - 192.168.1.255. This means that the 192.168.1 describes the network, and the final octet is used to describe the individual node. If you were to show this in the binary version, the first 24 bits would be used to describe the network (192.168.1) and the last 8 would describe the node.

| network                        | host      |
| ------------------------------ | --------- |
| 1100 0000.1010 1000.0000 0001. | 0000 0001 |

There are two ways to define how many bits are reserved for each section of the IP address. One is by just adding the number of network bits used, like this: `192.168.1.0/24`. This notation, either known as **slash notation** or **CIDR** (Classless Inter-Domain Routing) **notation** is what most network admins will use to define subnets.

The second way is to make a binary **subnet mask**. This means making a new 32 bit number that has all ones where the network is described and all zeros where the host is described. The same network as above, described this way would look like this:

`network: 11000000101010000000000100000000`  
`subnet:  11111111111111111111111100000000`

Translating this into decimal would look like this:

`network: 192.168.1.0`  
`subnet:  255.255.255.0`

We haven't talked about what a subnet is, but the basic idea is that it describes which computers are part of the local network versus computers which are part of an outside network. So if one node is trying to talk to another in the same subnet, it will talk to the other node directly, using the hardware address. We will do more with subnets and routing shortly.


#### Exercises!

> Have students take common numbers and convert them to binary and back. Also have them convert to and from hexadecimal.



### Subnets and Broadcasts

So what are subnets? A subnet is the way to define the local network. When a node wants to communicate with another node on the same subnet, they should be able to use MAC addresses because they're directly connected. When a node is trying to reach a node outside of its subnet, it is not directly connected so it must find a route to the other node.

Nodes that are connected to the same subnet can also use broadcasts to send packets to all machines in the local hardware system. There are different kinds of broadcasts, from DHCP (Dynamic Host Configuration Protocol) and ARP (Address Resolution Protocol) requests, to broadcasts for pushing new operating systems out over the network. Broadcasts are generally limited to the local subnet.

> Show some broadcasts and what they might do, including talking about what would happen if two nodes responded to a broadcast (like with DHCP)

### Switches vs. Routers

We have been talking about all local networks or **Local Area Networks** or **LANs** so far. Most times, a LAN is a collection of nodes that are connected via a switch. The most useful networks are ones that are connected to other networks. This would allow internal LAN nodes to talk to the **Wide Area Network** or **WAN.** An example of this would be the Internet.

For one node to communicate with another across the WAN, they need to be able to route to each other. The most basic way for routes to be set up is for a node to have a default gateway set. Once a node has a default gateway established, every request sent to a node that isn't in the local subnet is sent via the gateway. The internal node assumes that the gateway has routing information for any IP address it doesn't have local access to.

How does a gateway know how to get to a WAN IP address? Each node in a network knows which the identifier of the network it is directly connected to and the IP address of the default gateway (which must be directly connected to the network). A routers is a node on the network which also knows about some other networks. Gateway is an old term for router, although it also has other meanings. Networks can be thought of as trees. Each branch knows which leaves are on the branch and which aren't. This allows routers to be able to route requests quickly and easily.

> Draw this concept, as it can be hard to understand. Show how a router that is on the 8.4.0.0/16 network would send packets to a router that is on the 16.16.0.0/16 network. Draw this concept as a tree and very basic as if there is only a single path that works. Multiple paths aren't needed quite yet.


This is also a simplification, because networks are both trees and circles, so requests don't always have to go up to the root node before going down a new branch. Depending on how the networks are set up, requests from one node to another can take one of many routes, or even have the individual packets of a request follow different routes.


### How do machines communicate?

As I mentioned before, nodes communicate with each other using IP and MAC addresses. To understand how they talk when going across the WAN we will use the OSI (Open Systems Interconnection) Reference Model.

> Draw/explain the first four layers of the OSI model and how two machines on a local network would send packets to each other. Then do the same for two machines across the WAN. A good way of explaining is using the envelope analogy.

> Make sure the Physical, Data Link, Network, and Transport layers are all well described.


### How do machines use IP Addresses and Ports?

Once a packet makes it to the correct machine via the IP address, the machine will then look at the requested port and see if there is a program listening on that port.

What is a port? It is a number from 0 to 65,535 used so a machine on a network can direct different requests to different internal processes, much in the same way IP addresses are used to route network requests to different machines in a network or internetwork.

There are several common ports that programs run on, although there is no definition of what *must* run on any given port. A website can run on port 3 as easily as it can run on 3,000. The only restriction is that two services cannot run on the same port at the same IP address. What defines the defaults are the conventions of the programs and protocols.

#### Standard Ports

| Port | Program          |
| ---- | ---------------- |
| 80   | HTTP/Web         |
| 443  | HTTPS/Secure Web |
| 21   | FTP              |
| 22   | SSH              |
| 25   | SMTP             |
| 53   | DNS              |
| 3389 | Remote Desktop   |

By default, Chrome (or any web browser) will request websites from port 80 or 443. You can override the default, but it is there so there are fewer things for people to remember when typing in server addresses.

Internally, a port is a number that a program has reserved for itself to listen on. When a packet comes to a machine (via the MAC) and the machine opens it, it then looks at the IP address. If the IP address matches one of those belonging to the machine, and the transport-layer protocol (like TCP) matches one running on the machine, then the machine looks at the port to see which program should get the packet. From there, the program will continue to unpack the packet until it gets the info that it needs.

### How can multiple server programs run on a single machines

Because a machine can have multiple IP addresses and each address has a set of over 65,000 ports for each transport-layer protocol available, multiple server programs can run on a single machine. This can be done by assigning different ports to the two services, hosting on other IP addresses, or even putting some type of proxy in place that can look at incoming requests and route them to the correct program based on the info in the request.

> Have students try to set up at least 2 different ways of hosting multiple servers on a machine. Minecraft is a pretty good server to use, since it is all Java based and has both IP and port settings available in the server.properties file.


### Common Network Services

Most networks aren't set up fully manually, as we've been doing. There are several basic network services that most networks run. The two most basic are **DNS** and **DHCP**.

#### DHCP

**Dynamic Host Configuration Protocol** or **DHCP** is what networks use to automatically configure machines so they have network access. It normally will give out IP addresses, subnet information, default gateway information, and DNS settings. It can also be used to send more information as needed, such as network time info, IP telephony information, and more.

DHCP works by listening for broadcast packets on a subnet. When a DHCP server hears a broadcast packet for DHCPDISCOVERY it will offer an address to the machine. The machine then verifies the address with the DHCP server directly and if approved, the machine will then use the DHCP info given to it.

#### DNS

**Domain Name System** (ain't nobody use that name) or **DNS** is a network service that translates server names into IP addresses. This is also referred to as Name Resolution or DNS Resolver.

Without a DNS server, everyone would have to remember every IP address of every website they want to go to. This is not very useful, obviously.

DNS is a very large topic. DNS is the backbone of almost every aspect of Microsoft Active Directory and is generally the first service to check when there is a network outage. We will dive more deeply into DNS in week two.
