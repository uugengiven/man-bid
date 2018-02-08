## Active Directory ##

Most large networks need a method to authorize and authenticate users, computers, and services within the network. There are many different pieces of software that can do this. Microsoft's is called Active Directory.

Active Directory has many uses in a Windows network. It can do the basics of authenticating and authorizing users and computers. It can also hold extra information on users, such as addresses or phone numbers. It can hold extra information on computers such as location. It can be used to apply scripts and other policy objects. It can even be extended to hold information for other network services, such as email, to use for configurations.

At its core, though, Active Directory is a listing of objects shaped like a tree.

Before we get too deep into AD, let's install and use it a bit before going further.

> Have students follow the role installation and DCPROMO instructions in Exam Ref 70-742 [pp 4-12] to create a base forest and domain. 

> Have students create a user in AD Users and Computers to the domain, then log into the DC as that user. Have them try to make changes to AD at this point. Have them log back in as a domain administrator and add their account to the Domain Admins group membership.

> Have students add a desktop version of Windows (Windows 10) to their domain and log into the computer as a domain user.

### Domains ###

Active Directory (AD) holds objects in what it calls a domain. This is often synonomous with a DNS domain, but can have additional meanings in AD. Multiple domains that all share the same base (www.facebook.com and images.facebook.com share the same facebook.com base) are put into a tree within AD. The entire collection of domain trees is called a Forest.

A domain is generally thought of as a logical segregation of elements. It might make sense for a branch of Contoso on the east coast have a different set of users than the west coast branch. And each branch may even have its own collection of administrators that shouldn't be able to affect the other branch's users. Domains are how this separation is done, in AD.

Most AD setups have a single forest and tree, unless there is a special need for anything else. Even if a company has multiple branches, they will generally not need separate trees or forests. An example may look like this:

Tree: contoso.local
Sub-domain: west.contoso.local
Sub-domain: east.contoso.local

Each of these domains is in a single Tree, called contoso.local. East and West each are part of that domain. It would also be possible to have a setup like this:

Tree 1: west.local
Tree 2: east.local

In this case, the two domains still represent the same things (the East and West branches of Contoso) but they are no longer within the same tree.

It is also possible to have a mixture of trees with subdomains and multiple trees, and even multiple forests within a single AD configuration.

## Trusts ##

Earlier we discussed the different ways of laying out domains within separate trees or forests. While each layout gives a domain the ability to administrate their own users and computers, separating domains into separate trees or forests does affect one major component of Active Directory.

AD takes care of both authentication and authorization of users. Authentication meaning the user is who they say. Authorization meaning the user is allowed to connect to a given resource. Trusts are used in AD when one domain wishes to give authorization to a user that is authenticated by another domain.

In practice, this means that if there is a trust between east and west, an Administrator in east could give authorization from Alex in the west domain to access resources in the east domain.

If this trust is not there, the east domain cannot ask the west domain to authenticate any users, or even see which users or groups are part of the west domain to allow them authorization to resources.

In Active Directory, any domains within a forest automatically have a *two-way trust*. This means that domain A can authenticate for domain B, and vise versa. It is possible to also establish a *one-way trust* where domain A may authenticate for B, but not have B authenticate for A.

For most companies, a single forest is usually fine in AD. However, there is the concept of a Forest Administrator, who has full access to everything within a forest. If a company has a need to fully segregate administrative abilities, they may need to have multiple Active Directory Forests so there is not one admin on top of everything.

## DNS ##

You may have noticed that we've been using the word domain a lot, and that the example AD domains we have talked about look like normal DNS domains. That is because they are.

Active Directory uses DNS as one of the backbones of its service. The Domain Services Role will automatically install the DNS Role when AD is setup, as it requires DNS to be installed and functioning properly throughout the network. Computers that wish to authenticate users must have DNS setup properly. When an administrator adds a PC to the domain, the DNS must be setup properly so it can be properly updated. When AD tries to keep all of the servers updated with user/group information, DNS must be set up correctly to allow it to work.

Because DNS is the backbone of AD in terms of networking, DNS is often the first place to check when Active Directory type issues start to appear (such as users not being able to authenticate or file shares missing).

AD will take ownership within DNS of the domains it owns. It is important to not put in a real domain name, unless there is a true business need. For example, DCPROMO will allow you to list facebook.com as your domain. After that point, all DNS requests to facebook.com will go to your DNS servers. If someone were to try to get to their facebook at `www.facebook.com`, you would have to put in a valid entry into your DNS for www, or the request would fail. And any time Facebook did any kind of update to their hosting or IPs, those entries would have to be updated.

Most of the time, Administrators get around this by making their AD domain have a suffix that doesn't exist, like `.local`. So, `contoso.local` or `myapp.local` would be fine to use, as there are no top level domains that end in `.local`.

## Topology ##

Active Directory is run as a collection of **Domain Controllers** that provide access to AD. Domain controllers, as mentioned before, will run DNS along with AD authentication services, replication services to other domain controllers, and depending on the domain controller, will accept changes to the AD database (password changes, security updates).

These controllers are split into sites, which normally match a company's physical network. AD expects all domain controllers within a site to be able to freely connect to each other on the AD ports and will replicate information between all domain controllers within a site.

Between sites, domain controllers will need to be able connect on AD ports, but a specific connection can be defined. For instance, there may be a setup that looks like this:

| Site | Name | IP |
| ---- | ---- | -- |
| East | DC-1 | 192.168.1.10 |
| East | DC-2 | 192.168.1.20 |
| East | DC-3 | 192.168.1.30 |
| West | DC-4 | 192.168.5.10 |
| West | DC-5 | 192.168.5.20 |
| WestExt | DC-5 | 192.168.10.10 |

In this setup, DC-1, DC-2, and DC-3 would all be able to connect and replicate to each other. DC-4 and DC-5 would be able to replicate to each other. Any changes made on any of the domain controllers would then be replicated to any domain controller in the same site. Between sites, a connection would have to be setup between two of the domain controllers to allow intra-site replication.

By default, AD creates a default site link to try to allow replication across sites. However, it is possible that the domain controllers in East above cannot connect to the WestExt domain controller. In this case, specific site links would need to be made, one between East and West, and another between West and WestExt. This configuration would mean that any changes in WestExt would have to go to West, which would then send them along to East.

