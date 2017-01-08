# Basic Windows Permissions

Permissions are how servers and machines decide who is allowed to do what. The concept is simple but in practice it can get complicated. There are two aspects to permissions traditionally: **authorization** and **authentication**.

**Authentication** is verifying that the person is who they say they are. For this lesson, we'll be skipping over authentication and will assume that whatever way we are authenticating is working. Common authentication methods are Windows Auth, Basic, OAuth, and none. Yes, none is a valid authentication, which means that everyone who hits a certain resource is assumed to be the same person. Sometimes that is a guest or anonymous account, sometimes it is assumed to be a specific account.

**Authorization** is what we're going to go over today. Authorizing is deciding whether a specific account has access to perform a specific action on a resource. Actions depend on the resource; databases have different actions available to them then email, which are different than file resources.

###	Read, Write, Execute, Delete

The basic actions available in the file system are `Read`, `Write`, `Execute` and `Delete`. Read, write and delete are all self-explanatory. The execute permission says whether an account can run a file that is an `exe` or `executable`.

Often times the same users shouldn't have `Write` and `Execute` access, as if a person can compromise that account, they could use it to both write an unsafe executable to the machine AND make that program execute.



###	Applying permissions in windows

Applying permissions in Windows is mostly easy. There is a GUI method:

> Show setting permissions in the GUI via right click and then security.

There is also setting **ACLs** or Access Control Lists via Powershell. This is done by using the `ACL` noun in Powershell. Common ways of copying ACLs look like this: `Get-Acl -Path "C:\Dog.txt" | Set-Acl -Path "C:\Cat.txt"`

> Have students create two local users on their machines and then create two files. Have the students set each file to only have Read/Execute permissions on one of the users. Have students log in and out of the two users to see which files they can see.

> Next, have students create a folder, with user 1 getting Read and Execute permissions and user 2 getting read and write permissions. Have students then copy an `exe` file to the folder and try to run it with both users.

> Have students create user 3 with the same permissions as user 1. Have students create user 4 with the same permissions as user 2.


###	Permission groups vs users vs computers

Permissions are not only by individual user. Permissions can be set on groups, which users can be part of. Permissions can also be set on computers, which is useful when you providing access to resources over the network.

Most often, groups are the best way to assign permissions. Individual permissions become too cumbersome very quickly. Computers can be added to groups if it is important to allow access via computer account instead of a user account.

> Have students create groups to hold the permissions that they created earlier (on the two files and folder). Remove all individual user permissions and add the users to the correct groups so permissions still act the same way.

> Have students create user 5 to match the permissions (using groups) of user 1. Have students create user 6 to match the permissions of user 2.


###	Deny vs Allow

In Windows, Deny will always outweigh Allow. There are times when deny can be useful for setting permissions, however. Such as making a group that generally has access to a folder and a specific account needs to not have access to that folder. Instead of making two separate groups, one with and one without access to that folder, the group could be given Allow permissions to the folder while the individual account can be given Deny to that folder.

In the GUI, Deny is not always obvious. It will sometimes be hiding under the Advanced button in Security. If you are having issues where it looks like everything should be working but is not, look for something to be listed as denied in the permissions of that resource.

###	Ownership

Each file has an owner assigned to it. This is normally the account that created the file. The owner is given the permission to grant and deny other accounts permissions to the file or folder. This permission cannot be revoked. Ownership can be changed through the GUI, in the same screen as the normal permissions can be set. In Powershell, the owner is set with `Set-Acl` just as permissions are. There is a `.SetOwner` command that can be used.

> Have students change ownership of files using both the GUI and Powershell.

### RunAs and Impersonation

We glossed over **Authentication** earlier but there is an important aspect to authentication and authorization when talking about servers. Each server is a running program on a machine. This program runs under a certain account context. By default in windows, the account context is the Local System account. Any permissions given to Local System are also given to the server or `exe` that is running because that program is acting as that user.

It is important, therefor, for a server that needs to have specific permissions to run as a specific account. This can be done in several ways. The basic way is to select `RunAs` when going to run an `exe`.

> Show this happening by using explorer to show the files back from exercise one - one user can see the one file, the other user can see the other file

For most servers, though, they run as **Services** in Windows. In the Services view, there is a field specifically for the account context that the server will run as. We don't have to set this yet but as we start installing servers, we will have to make sure the context they run under is correct so the server has access to all of the files and folders and other resources that it needs to be able to run.

There are times when a user may have access to resources that the base account that runs a server does not. In this case, the server must **Impersonate** the account that has access. When the program is impersonating another account, it has all of the access that the account it is impersonating has.
