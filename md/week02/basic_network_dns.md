## Routing outside the network

Our networks so far have been self contained, for the most part. We have been working on *LANs*, but now we need to be able to connect one LAN to another so they can talk to each other.

To connect two LANs together through a WAN, there must be a `route` from one to the other. In each LAN that is connected to a larger WAN, there is at least one node that works as a router, sending packets from one network to another.

In the case of a home or small office network, there is a single router that translates messages from the internal LAN to the external WAN. In larger networks, there may be several LANs that can talk via different nodes and one or more of the LANs may be connected to the larger internet.

> Draw this concept. First draw it with a small office with a single subnet internally. Then draw it again with multiple subnets with only one that connects to the internet. Then show how there may be one route that connects to a subnet and then a separate route to go to the internet.

At the most basic, Routes tell a node where to forward messages that are for a network outside of its local subnet. It is possible for there to be multiple routes, possibly having separate nodes taking care of routes to different LANs.

> 


## Naming systems and DNS


We've been talking about how machines talk via IP addresses, but that is not very useful for humans. Also, there are many times when an IP needs to change, and going in to every configuration and program that uses that IP and updating it is a big pain.

Our answer to that is `DNS`.

DNS lets us use custom names that then point to one or more IP addresses, another DNS name, or possibly to other information. DNS is also used to broadcast certain network services that are available to networks or domains. An example of this is an MX record, which marks which names/IPs are available to receive mail to a domain.

How does this work?

> Explain how email software looks up where to send an email message when one is sent. Include the dns look up for the MX record, packaging the email into an understandable format for the receiving server, then contacting that server and pushing the email to it.



## WINS (heh)
## Network to network routing
## Subnetting, CIDR, /notation, more
## Routing, Gateways, touch on BGP
## How multiple computers get to the internet behind a single IP (NAT)
