# Microsoft File Shares

One of the most common needs in small offices is shared file storage. Before the Internet was so ubiquitous, a local file share was used. This is still a very common need in businesses, as services like **OneDrive** or **Google Drive** do not work if there is an interruption to the internet.

Microsoft provides another role in Windows Server that creates `SMB` file shares.

## SMB

SMB or `Server Message Block` shares are a cross platform way to share files. SMB itself is a protocol for file sharing. The Microsoft version of SMB provides some useful features:

 * Network browsing
 * Printing over a network
 * File, directory, and share access authentication
 * File and record locking
 * File and directory change notification
 * Extended file attribute handling
 * Unicode support
 * Opportunistic locks

## Installing File Shares Role

> Depending on the version of Windows Server used, do a step-by-step walk-through of installing the File Share Role

The role itself does not actually create file shares, nor is it required to create shares. However, it gives several useful management tools for file shares. It also adds the ability to do replication to remote sites for failover.

## Shares

Shares themselves are folders that are accessible over the network. The name of the folder may or may not match what name it has on the network. Windows and most other operating systems have the SMB protocol pre-installed and can go to network shares with no special configuration. Even smart phones can access shares that are on the same network that they are on.



## Set up a share

> Walk through setting up a share in three different ways. One, clicking on a folder and selecting share. Two, creating it through the File and Share Management Console. Third, show how to create a share using `New-SMBShare` in Powershell.

Once a share is created, any computer on the network can access the share, as long as it has the authorization. To get a network share, the easiest way is to open an Explorer window and type `\\network_name_of_server` - that will list all of the visible shares to the currently authenticated user. At this point, that user will be **Guest** or **Anonymous**.

> Have students set up shares of their own, using each method. The students should be able to browse the shares, copy files to them, and edit files in the shares.

## Share Permissions

File shares use the same permissions that we used with files and folders before. The permissions model is the same whether it is a local file or a networked file. The difference is the share has its own set of permissions, along with the files and folders. Therefore it is possible to have access to a share without having read/write access to a folder, so a specific folder (or file) in the share will be unavailable.

We are going to test this, now, similarly to how we tested our folder permissions before. First, we will have to make several user accounts on the server machine. These accounts are separate from the machine that is accessing the shares. Then, set up shares with the following attributes:

> * A share with full read/write for everyone on the share, but read only for one specific user on the folder being shared
> * A share with read/write for only one user in the share permissions, full read/write on the folder being shared
> * A share with full read/write for everyone on the share, but read only for everyone in the folder
> * A share with full read/write for everyone on the share, read/write for everyone in the shared folder but inside this folder, have a file that can only be read by one user, and a second file that can only be read by another user (see week 01's basic permissions for a similar setup)

With this setup, we can test how shares alter our view of shares, depending on which user account we use to connect to them.

> Have students test copying and editing files and folders with these different share setups. Discuss uses when one permission style might be preferable over another when using shares (using share permissions as the primary security or file/folder permissions as the primary security).
