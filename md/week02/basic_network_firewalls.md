> This section is highly dependent on the hardware used at the school. Cisco, Ubiquiti, SonicWall, Juniper, and others are all possible hardware providers for firewalls. All of these use the same basic ideas but each has their own syntax for creating rules. Once hardware is decided, this section should be updated with the proper syntax for the school. **It is also possible that this syntax will change school to school.**

## What is a firewall

A firewall is often a piece of hardware that will inspect all packets going from one network to another and potentially change or even remove packets based on internal rules. This can be very much like a router and quite often, routers and firewalls are in the same hardware and configured using the same interface. For our purposes, we will generally be considering a firewall as something that **allows or denies network traffic based on a source or destination IP address or port.**

In our previous network drawings, we have shown requests from a machine go through a router and to another machine, then have responses come back. Often, at the router, a firewall will also be there and may block traffic if there are rules to do so.

> Draw this, showing a web server behind a router. Show what happens if there is an allow rule and then what happens if there is a deny rule on port 80. Then draw a version that shows the web server behind NAT and how the router does the NAT translation while the firewall still makes decisions based on source/destination address and port. Finally, show how configurations would fail if the router and firewall don't match.

Right now, we are concerned with the basics of routing and firewalls. We only care about IP addresses (OSI layer 3) and ports (OSI layer 4). When we talked of switches before, they generally work on Layer 2. As we continue to go deeper into firewalls we'll discuss what a Layer 7 (or sometimes 4/5/6) firewall can do that is different. This is not that time.

## Firewall rules

Firewall rules are all individual lines of logic that are run, one at a time, to decide if a certain packet should get through or not. Each rule results in either **Allow** or **Deny**. The first rule in the list that matches a packet decides whether that packet will be allowed or denied.

ex: A packet from 192.168.1.10:80 that is going to 10.0.1.5:80 comes through the firewall. The rules on the firewall are as follows:

| Source IP    | Source Port | Dest IP  | Dest Port | Allow |
| ------------ | ----------- | -------- | --------- | ----- |
| any          | any         | 10.0.1.1 | 53        | allow |
| any          | any         | 10.0.1.5 | 80        | allow |
| 192.168.1.10 | any         | any      | any       | deny  |
| any          | any         | any      | any       | deny  |

The request above will test rule one and not match. Then it will test rule two and match, therefore being allowed. Even though rule three specifically calls out 192.168.1.10, because rule two is run earlier, rule three is never reached for this packet.

## How to log into a router/firewall

> Show how to connect to the school-provided firewall both via terminal/ssh and via the GUI.
> Show how to connect via console cable as well.

## How to configure a firewall/router

> This section is dependent on the hardware - once hardware is chosen, showing a step-by-step procedure here would be very useful.
> * Show how to set up port translation via GUI and ssh
> * Show how to set up firewall rules via GUI and ssh
>  * Example rules should include a rule that blocks every request to a port, every request to a specific address, every request from a specific address, a rule that allows all traffic from a specific address to a specific address


## Setting up our firewall

The two different network services that we worked on earlier are `DNS` and `File Shares`. DNS servers use port 53. SMB Shares run on ports 445 or 139. We also used Minecraft earlier as a test to show another type of server.

We want to do the following:

> Take two client machines with two different IP addresses. Allow one to access DNS and File Shares through the firewall. Do not allow the second client address to access those network services.
> Run two separate Minecraft servers, one on port 60,001 and one on port 60,002. Minecraft defaults to port 25,565. When client 1 requests to connect to the server on port 25,565, it should have its port translated to 60,001 and be allowed through the firewall. When client 2 requests to connect to the server on port 25,565, it should have its port translated to 60,002 and be allowed through the firewall. Both clients should be connecting to the "same" server as they can see externally, but both be connecting to different servers.
