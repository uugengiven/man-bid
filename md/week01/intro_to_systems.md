# Intro to Systems #

### What do servers do? ###

Servers are programs that are accessed via a network. This can be something such as a web host that gives out websites to computers on the internet. It could also be an internal SQL server that gives data out to other internal servers for them to use. A server's job is to respond to requests over a network.

### How do they do it?

Each machine runs one or more server programs that listen for requests on a network and respond with appropriate information. This information could be text, data, or even a physical action in the real world, like unlocking a door.

Often servers connect to other servers in a chain to perform what may seem like a simple action. In an example of unlocking a door with a click of a button on a phone, there is a server that listens to internet requests, which in turn talks to a server that verifies authorization, and then talks to a server that is physically connected to the door, which then activates a relay through a physical port (interface) on the server.

A single physical machine does not have to host only a single server to do one specific task. Each layer of a machine, from Physical or Hypervisor to OS to Container to Server Software can usually host multiple servers beneath them. A Physical or Hypervisor layer can host multiple OSes. An OS can host multiple server software programs or containers. Server software can often host multiple instances of whatever they do: SQL Server can host multiple databases, IIS and Apache can host multiple websites.

> Draw a small example website with a back-end database, and how a client would connect to get the website information. It may also be useful to show images/content coming from a content distribution network (CDN) as well in this example.

### What is the role of a System Administrator?

A system administrator is responsible for keeping both the software and possibly the physical hardware running so systems can continue to listen and respond to requests in a timely manner. System admins are also generally responsible for keeping the hosting software (OS/Database/Email/Web) up to date to prevent security issues. Many times, System admins are also responsible for working directly with developers to help build a proper release plan to keep custom built software up to date as well.

As much as possible, a system administrator should be keeping components running so everyone else in a company can continue to do their jobs. Therefore, an admin should be testing updates, verifying work, and making sure that everything they do has as little user impact as possible. There are many methods of doing this, and each company will have a different thought behind how it should be done, but this is one of the key aspects of a good system administrator. In other words, the less visible a system admin is, the better.

### Servers (Physical Hardware vs. Server Software)

When we talk about servers, we will sometimes mean the logical software that is running, such as SQL Server or Apache. Other times, we will be talking about the physical machine that is hosting any number of virtual servers. System admins are responsible for watching individual components of a server, but also watching how the system performs as a whole. A system admin is responsible for troubleshooting performance issues, whether the issue stems from improper use of the server software to inefficiencies in the software itself to potential hardware or network issues.

The physical side of a server includes its hardware such as CPU and RAM, but also its connections to other core hardware, such as shared storage or networking hardware. Each of these is generally considered to be on the hardware side of a server. The hypervisor straddles the line between hardware and software but is often included in the physical layout of a server.

The software side of a server is generally from the OS or Operating System down to the actual running server software. This can include containers if they are used, connections to networks, and also includes permissions to resources.

### What kind of work is expected

System administrators are expected to be able to support almost any software that a company may want. That could be well-known software, such as Microsoft Office, MS Exchange, Apache, or MySQL. It could also be little-known software or even software that doesn't exist yet. There is no way for a system administrator to know how to support all of the software that they will be expected to support.

For this reason, it is important for system administrators to know *how* servers and networks work in general, rather than knowing explicitly how to work in Sharepoint or SendMail. Throughout this course, we will cover specific software, but not with the intent to learn that software completely. Rather, you will learn how to support any software by using certain software as examples.

Ultimately, at the end of this program you should know how servers work, how networks work, and how they should work together. And you will know how to troubleshoot problems, find where issues are occurring, and how to find answers to fix the problems. Finally, you will learn how to utilize the most important tool you have, web searches. You will not leave here knowing how to fix every single issue with every piece of software we touch. Rather, you will know enough to know how to make educated choices in debugging and searching to fix problems, no matter what the software is.
