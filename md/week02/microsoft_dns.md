## Set up MS DNS

> This lesson requires a Windows Server install with the DNS role not yet installed. The server should not be on an AD Domain and should not be a domain controller.

Microsoft offers a DNS server as part of the base roles in Windows Server. What we have been calling servers, Microsoft will often call a Role for Windows. There are multiple roles available for Windows Servers, from Web Servers to File Shares to DNS and more. In Linux we would call these each individual servers. This is a naming convention only, it does *not* change how we think about and troubleshoot these services.

We are going to first install the DNS role into Windows and then use this role to create our own DNS server with its own responses.

> Depending on the version of Windows Server used, a step by step install for the DNS Role should be provided here.

## DNS Tree Structures

DNS is structured as a tree. This tree is reflected in the DNS management console. The first level displayed in the tree marks all of the domains that this DNS server will respond to. While you could add `com` to this list, it would then respond to every single request that ended in `.com` if possible.

In our networks, generally the different domains listed here will be things like `example.local` or `mywebsite.com`, and not a TLD.

## Primary vs. Secondary domain

The domains listed in this management console are all of the ones the DNS server knows that it is responsible for responding for. However, there are two types of domains that can be listed here.

One is a `Primary Domain`, which means this DNS server saves and receives changes, and controls all of the aspects of the domain. This is a Microsoft designation, not a general part of DNS.

The other is a `Secondary Domain`, which means another DNS server saves and receives changes and this server is copying another DNS server.

In general, we will be making `Primary Domains` in our DNS server, as we want to be able to make changes and add new A records, or MX records, or any other kind of records we wish.

## Creating a domain

> Have students create a new primary domain, called theirlastname.local and create a few A Records for this domain. Have an IP address for a website available and have them create an A Record for www.theirlastname.local and have it point to this address. Then have them change their local primary DNS resolver to point to their DNS server. From here, do the following tests:

> * Have them do nslookup from the command prompt against their own domain. Make sure it responds with the A Records they expect it to.
> * Have them go to www.theirlastname.local and verify that it loads up the website provided.
> * Have students try to go to each other's websites; explain why it does not work.
> * Have students add www.facebook.com or another real site to their DNS and have it point to the provided website. Then have them go to the site and see how they can no longer get to the real www.facebook.com
> * Have students add different types of records, including CNAME and MX, and then have them look them up via nslookup

### nslookup

`nslookup` is a helpful command in Windows and other operating systems that allow for testing of name servers. By using nslookup, users can look up all different types of DNS information. nslookup can be used in command or interactive mode. In command mode, you can send it all of the commands and switches, like this: `nslookup www.google.com` will look up the A and AAAA Records of www.google.com. In interactive mode, you can do multiple lookups, switch between query types, and more.

One important thing to remember about nslookup is that it interrogates the DNS server, and it will not examine the `hosts` file, which is a local reference for A and AAAA Records. Normally `hosts` will only include an entry for `localhost` that points to 127.0.0.1.

nslookup is an invaluable tool in debugging network issues. You will be using it all of the time in this class and should become comfortable with reading its results and using it to request DNS info.

## Create DNS Records via Powershell

Almost every part of Windows server configuration can be made in Powershell. DNS is no different. By using `Add-DnsServerResourceRecord`, students should make several entries for their DNS server. Scripting DNS entries, or any other setup, is very useful when deploying new servers or automating deployments.

> Have students create and delete multiple entries in their domains using Powershell only. Have them watch in the management console as they make changes to see when they do and do not have to refresh the window for up-to-date information.

## DNS Forwarders

What happens when you make a DNS query to a server that does not own or know about a given domain? The DNS server needs to know which DNS server it should check for unknown domains. This works very much like a gateway in networking.

Forwarders are specific DNS servers that are the next server to ask. Often these are the ISP DNS servers that the network is using. However, there are times when it makes sense to use other DNS forwarders.

If no forwarders are set up, the DNS server will use the **root hints** that come pre-installed with Windows. These will generally work, but best practice is to use a next hop DNS server to prevent the root servers from being overloaded with unnecessary requests.

## Caching

Most DNS servers, Microsoft's included, have caching built in. This means that any request that comes in will be stored briefly so if other machines make requests for the same address, the DNS server can return the answer more quickly.

Caching is an important part of making sure large scale networks can function quickly. Many services allow for caching, including DNS, web services, and more.
