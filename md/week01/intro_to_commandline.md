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

You will learn more about these commands as we go through the weeks.

#### Filtering on Get

We used filtering earlier when doing `Get-ChildItem` and `ls`. Filtering is an integral part of Powershell and it has its own syntax for more in-depth filtering than what we have done with `ls`.

Powershell filtering is performed by piping your output to a `Where-Object` command. Inside this command, you can then set up your filter to only give the results that you want. Here is an example of a full statement:

`Get-ChildItem | Where-Object {$_.name -eq test.txt }`

This will list all files where the name is text.txt. This specific example isn't very useful, but the filter allows us to filter on more than just the name. The filter works like this:

`Where-Object{ [object.property] -comparitor [value] }`

Depending on the type of object, there can be any number of properties to compare against: name, date, date accessed, email address, group membership, size. The value can be any value that you want to compare against. Finally, the comparator is from a list of accepted Powershell comparisons, listed below:

| Comparator | Meaning |
| --- | --- |
| -eq | The object property equals the value |
| -ne | The object property is not equal to the value |
| -lt | The object property is less than the value |
| -gt | The object property is greater than the value |
| -contains | The object property (as an array) includes the value |
 | -like | The object property matches the value and the value can contain wildcards |

There are many more comparison operators in Powershell but these are the basic and generally most useful ones.

###	Specific Powershell commands

Let's go through some common tasks you may do via Powershell.

#### Set networking info

To set an IP, Subnet, or other networking information, you'll need to use several commands to do so. First, you will need to get the adapter id for the network device you want to change. You can do that with `Get-NetAdapter`. This will list all of the network adapters on the machine you are on.

Each adapter has an ifIndex, which is needed in the next step, where you can get or set the IP info.

Use `Get-NetIPAddress -InterfaceIndex ` and then the ifIndex number to see what IP is already set to the machine. You can then use `Set-NetIPAddress` to change the IP, `New-NetIPAddress` to add another IP, or `Remove-NetIPAddress` to remove an existing IP.

> Have students set a new IP and verify that it works, then remove that IP.

#### Get computer info

Powershell uses WMI (Windows Management Instrumentation) objects to look at machine hardware. There is a wealth of information available via WMI and it is important to see the basics of it.

[WMI Reference](https://msdn.microsoft.com/en-us/library/aa394572(v=vs.85).aspx)

> Have students find out how much space is left on their drives using WMI. The command they can use is something like: `Get-WMIObject win32_volume | fl driveletter, freespace`

#### Get file info

We have used `Get-ChildItem` to get a list of items in a directory. We can also use `Get-Item` to get a single item. Using `Get-Item` by default only shows certain information. To see the full list of everything that `Get-Item` returns, add `| Format-List -property *` to see everything that is tracked on a file.

You would then assume `Set-Item` would let you change these values. While `Set-Item` is a real command that does set values, it specifically is disabled when using files. Almost every other aspect of Powershell can use `Set-Item`, however.

###	Reading/Writing text files

What if we wanted to read or write what is in a file then. Especially write a file if we can't use `Set-Item`. For that, we use `Set-Content` and `Get-Content`.

> Have students create a text file using notepad. Have them print the contents of the file using `Get-Content`. Then have them put new text into the text file using, first replacing with `Set-Content` then by appending using `Add-Content`. Try to have students figure out how to append before explaining.

###	Powershell profiles

As you become more comfortable with the command line and doing system work in general, customizing your workspace will become very important. Powershell has very nice customization available through your Powershell Profile. This profile lives in your documents folder, inside `%USER_PROFILE%\Documents\WindowsPowershell\Microsoft.Powershell_profile`

By default, this file doesn't exist so nothing is loaded. If, however, you create the WindowsPowershell folder, then type in `notepad $profile` you will be taken to a blank profile that you can begin to fill in with useful variables and functions.

We haven't yet gone over what variables and functions are in Powershell yet, but we will soon. In the meantime, let's walk through creating a function that will help us download files from Youtube.

> Walk students through Get-YoutubeMP3 by using youtube-dl. This will require students to get youtube-dl working on their system and put it in a place that they can access. An example of Get-YoutubeMP3 is available at my [Github](https://github.com/uugengiven/PowershellProfile)
