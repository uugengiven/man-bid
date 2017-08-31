## Routing outside the network

Our networks so far have been self contained. We have been working on *LANs*, but now we need to be able to connect one LAN to another so they can talk to each other.

To connect two LANs together through a WAN, there must be a `route` from one to the other. In each LAN that is connected to a larger WAN, there is at least one node that works as a router, sending packets from one network to another.

In the case of a home or small office network, there is a single router that translates messages from the internal LAN to the external WAN. In larger networks, there may be several LANs that can talk via different nodes and one or more of the LANs may be connected to the larger internet.

> Draw this concept. First draw it with a small office with a single subnet internally. Then draw it again with multiple subnets with only one that connects to the internet. Then show how there may be one route that connects to a subnet and then a separate route to go to the internet.

At the most basic, Routes tell a node where to forward messages that are for a network outside of its local subnet. It is possible for there to be multiple routes, possibly having separate nodes taking care of routes to different LANs.

> This is mostly a review from week one. However, this must be covered again (and again and again) as each week, new questions will arise from students as their understanding grows


## Naming systems and DNS


We've been talking about how machines talk via IP addresses, but that is not very useful for humans. Also, there are many times when an IP needs to change, and going in to every configuration and program that uses that IP and updating it is a big pain.

Our answer to that is `DNS`.

DNS lets us use custom names that then point to one or more IP addresses, another DNS name, or possibly to other information. DNS is also used to broadcast certain network services that are available to networks or domains. An example of this is an MX record, which marks which names/IPs are available to receive mail to a domain.

How does this work?

> Explain how email software looks up where to send an email message when one is sent. Include the dns look up for the MX record, packaging the email into an understandable format for the receiving server, then contacting that server and pushing the email to it.

`DNS` is designed to work like a tree. Each node has as many branches as needed. There are many common root nodes, such as `.com` or `.net` that you are probably familiar with. These are called *Top Level Domains*. This means that the `.com` node has branches for `google.com` and `microsoft.com` and `facebook.com` and any other .com name out there. From there, the `google.com` node may have other branches, such as `mail.google.com` or `docs.google.com` while the `microsoft.com` node may have separate branches, such as `onedrive.microsoft.com` or `xbox.microsoft.com` or even `www.microsoft.com`.

The way this works is each node has a server responsible for answering DNS requests. And so, when you type www.facebook.com into Google Chrome, it will break each node out and ask, in order, to find the next hop, starting from the furthest right and moving left.

 * Chrome requests a dns lookup for www.facebook.com
 * The look up program goes for the first node, looking up the `com.` node first and getting the address of the `com.` name server
 * The look up program then sends the second node name to the first node, asking for the `facebook.` name server and getting that address
 * The look up program then sends the third node name to the second node address, asking for the `www.` **A Record** and getting that address (which is the final address)
 * Chrome requests the index page from the address returned by the DNS query

Because of the way DNS trees work, each domain has effectively unlimited numbers of subdomains as each node of the tree can act as a full name server if needed.



## WINS

There is another technology that is Microsoft specific, called `WINS`. This technology fills in most of the role of `DNS` for older Microsoft services, by changing computer names into IP addresses. WINS does have more limitations and is only used in older Microsoft environments. It is not something that you should need to deal with on a day to day basis but knowing that it exists is useful. We will cover it a bit more when discussing specific Microsoft servers and services.

## Externally Routable IPs

The IPv4 IP group has a few types of special IPs. There are groups of `non-routable` IPs. There are different types of non-routable IPs, one is the IP networks that are reserved for private/internal networks listed below:

 * 192.168.0.0 /16
 * 10.0.0.0 /8
 * 172.16.0.0 /12

Other common `non-routable` IPs are the local loopback addresses `127.0.0.0 /8` and the `169.254.0.0 /16` network used for when DHCP requests are not answered.

In practical terms, it means the private networks are what are used when setting up internal LANs.

## How multiple computers get to the internet behind a single IP (NAT)

There are not enough IPv4 addresses to have an individual address for every machine that needs an IP. To start to combat this, `NAT`(aka `One to Many NAT` or `IP masquerading`) is used to allow an internal network to connect to an external network through a single IP. This means an office with 100 computers can all connect to Google or Hotmail through a single externally routable IP.

> Draw a packet going from an internal network to an external network. Explain that the external network has to know how to send information back to the internal network.

When a packet goes from one computer to another, much like a letter through the mail, it has a return address. This address is what the machine sends responses to. If the original machine has a `routable` address then the return address is its actual address. If a `non-routable` address is sent instead, the response will be lost as there will be no route.

What a router with `NAT` set up does is rewrites a packet. This means a packet from `192.168.1.5` to `8.8.4.4` would get rewritten at the router to give the router's routable address to pass along, and the packet would now say it is from (for example) `210.12.9.33` to `8.8.4.4`. When `8.8.4.4` responds, the response will reach the router, at which point the router should rewrite the packet again so it says from `8.8.4.4` to `192.168.1.5` then deliver the packet to the local machine.

> Draw this interaction at the router, showing the packet being rewritten both directions.

What happens when two separate machines internally want to go to the same external machine and are waiting for responses?

The router does not only rewrite requests but keeps track of all requests and their original state. Then it checks each packet coming in to see if it matches a packet that has gone out and is awaiting a response.

And, because packets use both IP and Port (like zip code and street address), it is possible for routers for larger networks to use rewriting of the Port as well to keep track of many machines sending packets to the same external addresses.

## Routing, Gateways, touch on BGP



## Questions for class

What is the difference between typing `ping 10.0.0.5` and `ping test.local` assuming that test.local points to 10.0.0.5

> None, other than having to do a DNS lookup on test.local before running the ping command
