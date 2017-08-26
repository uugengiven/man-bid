# Microsoft File Shares

One of the most common needs in small offices is shared file storage. Before the internet was so ubiquitous, a local file share was used. This is still a very common need in businesses, as services like **OneDrive** or **Google Drive** do not work if there is an interruption to the internet.

Microsoft provides another role in Windows Server that creates `SMB` file shares.

## SMB

SMB or `Server Message Block` shares are a cross platform way to share files. SMB itself is a protocol for file sharing. The Microsoft version of SMB provides some useful features.

 * Network browsing
 * Printing over a network
 * File, directory, and share access authentication
 * File and record locking
 * File and directory change notification
 * Extended file attribute handling
 * Unicode support
 * Opportunistic locks

## Installing File Shares Role

> Depending on the version of Windows Server used, do a step by step walk through of installing the File Share Role

The role itself does not actually create file shares, nor is it required to create shares. However, it is gives several useful management tools for file shares. It also adds the ability to do replication to remote sites for failover.

File shares use the same permissions that we had done with files and folders from before. The permissions model is the same whether it is a local file or a networked file. The difference is the share has its own set of permissions, along with the files and folders. Therefore it is possible to have access to a share without having read/write access to a folder, so a specific folder (or file) in the share will be unavailable.
