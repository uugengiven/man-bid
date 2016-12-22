# Intro to Systems #

### What do servers do? ###

Servers host programs that are accessed via a network. This can be something such as a web host that gives out websites to computers on the internet. It could also be an internal SQL server that gives data out to other internal servers for them to use.

### How do they do it?

Each server runs one or more programs that listen for requests on a network and respond with appropriate information. This information could be text, data, or even a physical action in the real world, like unlocking a door.

Often servers connect to other servers in a chain to perform what may seem like a simple action. In an example of unlocking a door with a click of a button on a phone, there is a server that listens to internet requests, which in turn talks to a server that verifies authorization, and then talks to a server that is physically connected to the door, which then activates a relay through a physical port on the server.

A single physical server does not have to host only a single program to do one specific task. Each layer of a server, from Physical or Hypervisor to OS to Container to Server Software can usually host multiple items beneath them. A Physical or Hypervisor layer can host multiple OSes. An OS can host multiple server software programs or containers. Server software can often host multiple instances of whatever they do: SQL Server can host multiple databases, IIS and Apache can host multiple websites.

### What is the role of a System Administrator?

A System Administrator is responsible for keeping both the software and possibly the physical hardware running so systems can continue to listen and respond in a timely manner. System Admins are also generally responsible for keeping the hosting software (OS/Database/Email/Web) up to date to prevent security issues. Many times, System Admins are also responsible for working directly with developers to help build a proper release plan to keep custom built software up to date as well.

### Servers (Physical Hardware vs. Server Software)

When we talk about servers, we will sometimes mean the logical software that is running, such as SQL Server or Apache. Other times, we will be talking about the physical machine that is hosting any number of virtual servers. System Admins are responsible for watching individual components of a server but also watching how the entire system performs as a whole. A System Admin is responsible for troubleshooting performance issues, whether the issue stems from improper use of the server software to inefficiencies in the software itself to potential hardware or network issues causing performance problems.

The physical side of a server includes its hardware such as CPU and RAM, but also its connections to other core hardware, such as shared storage or networking hardware. Each of these is generally considered to be on the hardware side of a server. The Hypervisor straddles the line between hardware and software but is often included in the physical layout of a server.

The software side of a server is generally from the OS or Operating System down to the actual running server software. This can include containers if they are used, connections to networks, and also includes permissions to resources.
