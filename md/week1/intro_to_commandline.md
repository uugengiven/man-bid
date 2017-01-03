# Intro to command line

Throughout this course and your sys admin life, you will be in the command line all the time. After this course, you may be in the command line for most things that you do on your local computer too.

What is the command line? It is a text only program that you can use to run programs, change settings, make scripts, and do almost anything that you would need to do to keep machines up and running properly.  It is often the most concise and fastest way to look at and fix problems.

However, the most important aspect of the command line that every command run here is repeatable on any other machine. Once a script is written and runs correctly, it should run on any other system that has the same OS/server software.

**Repeatability** is extremely important to system administrators, as it is what lets us actually do our jobs. If every machine we touch has unique settings then it is hard, if not impossible, to diagnose problems.

###	Powershell basics

What is Powershell? It is Microsoft's version of Bash. What is Bash? We'll cover that in two weeks...

Powershell is a command environment that has a lot of built in commands along with the ability to create your own commands. It comes with the ability to declare variables, create loops, use arrays, build functions, and more. This will let us create logic within our scripts.

Powershell, like other shell languages such as Bash, Powershell allows you to take the output of one command and pipe (**|**) it to another command. In this way, you could have Powershell get a list of files and pipe it to a command that will copy each of those files to a new directory. Or delete the files. Or put their name into a text file. It depends on what you want to do.

###	Folders – navigating, creating, deleting

The first part of working within the shell environment is to begin navigating. When you are working in the shell, you are generally working within a folder on your machine. You can change which folder you are in and move around the contents of the computer much like you would by going through the file explorer.

The basic commands for this are:

| Command | Aliases | Description|
| --- | --- | --- |
| `Get-ChildItem` | dir, ls | Get a list of all files and folders within a directory |
| `Set-Location` | cd | Change directory or set the location in which your shell is running |
| `Copy-Item` | cp, copy, cpi | Copy an item or items |
| `Remove-Item` | del, rm | Delete an item or items |
| `New-Item` | mkdir | Make an item or directory |

> Create a folder in your documents folder, then create a sub folder in this new folder. Then delete both folders. Move to the Windows/System32/drivers/etc folder and list all of the items there. Copy the hosts file to your desktop.

`Get-ChildItem`, or `ls`, as you will want to use the unix style commands as often as possible, can list anything in the current directory. It can also list a subset, by giving it a filter. This filter is added to the end to tell it which files you want to see. An asterisk is used as a wildcard in these filters.

| Filter | Result |
| --- | --- |
| `*.*` | All files with a . |
| `d*` | All files that start with a d |
| `*.txt` | All files that have a .txt extension |

Filters can be separated by commas to include all files that meet at least one filter definition. You could write `ls *.txt,*.docx` to get all of the text and Word files in a directory.

`Get-ChildItem` can take multiple other parameters, but one very useful one is `-recurse`. Other commands will use this parameter as well. This will tell the command to look in the current folder and any subfolders that are inside the parent folder.

> Have everyone list all pictures within their documents folder. Have them explain how they made sure to get each picture type and not include any non-pictures in their results
> Have them try to do other filters. See if anyone can filter to see all of the files on their system that are older than 10 days.

###	Files – create, delete, list

Files and folders are treated very similarly in Powershell. Both use `Get-ChildItem`, `New-Item`, `Remove-Item` and their aliases. Creating files from the shell is generally reserved for text documents, although there may be times when you want to create another type of file.

All commands that output text can have that text saved to a file. This is done by piping to the `Out-File` command or using the short code `>` to do the same thing. This would look like `ls | Out-File sometext.txt` or `ls > sometext.txt`.

> Create a new text document in the documents folder that holds a list of all image files in the documents folder.

###	Basic Powershell Commands

So far we've been using some of the basic Powershell commands (or **cmdlets** if you're really into Powershell). There are more than I would want to list here but they all have a similar format. It is Verb-Noun. The Verbs are actions such as `get`, `set`, `remove`, `new`, `copy`, and so on. The verbs are somewhat universal. Whether you are working on a file, a user, or an email address, you will use commands such as `get` or `set`.

The nouns are domain specific. This means that the words used for files and folders are different than the ones used for users which are different than the ones used for email. Each domain has its own set of nouns that are used. `Location` and `Item` are nouns used in files and folders. `ADUser` is a noun for usernames. `ActiveSyncMailboxPolicy` is a noun for email in Exchange.

If you are unsure how to use a Powershell command, you can always use `Get-Help` for an explaination of the command and examples of how you can use it. Each Powershell command has its own help available. To see what is possible with `Get-ChildItem` you would type `Get-Help Get-ChildItem`.

#### Non-Powershell Commands

There are commands that were written pre-Powershell that are still used in the shell. Small commands such as `ping` or more complex ones such as `netsh` are not Powershell commands but programs that run via the shell. They have command line interfaces and can be invoked via scripts. There are a collection of built in commands in Windows. Often specific commands are added by server software to help control the software via the command line.



###	Specific powershell commands

 * 	Set networking info

 * 	Set computer info

 *	Set file info

###	Reading/Writing text files

###	Powershell profiles
