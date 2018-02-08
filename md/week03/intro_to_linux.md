# What is Linux (an OS Primer)

Similar to Windows, Linux is an **Operating System** that runs on physical and virtual machines. Linux has some important differences to Windows, the most obvious being that it is open source. This means in theory, anyone can add to the Linux codebase and add new features. A more important different, from a System Administrator standpoint, is that Linux is written primarly to be interacted with through the command line. You can read more about Linux [here](https://www.linux.com/what-is-linux).

## Interface

Being primarily command line based, Linux has several advantages over Windows. Being text based means it is very easy to automate setup and configuration of Linux machines. It also means that remote administration is easier, especially over slower networks, because only text is being transferred, not a full graphical interface.

It is possible to add a GUI on top of Linux. X11 is a very common GUI or Windows Manager, although there are several other GUI packages available, such as Gnome, KDE, or Unity. While using Linux on a desktop or laptop, one of these Windows Managers would normally be used, but for servers, the interface will be the command line shell.

## Software

Linux has a multitude of software availble. Most software on Linux is available through package managers. `apt-get` is the most common used, athough there are many other package managers. These managers often have their own repositories of software. This means that a known good copy of a package is available from a trusted source in Linux. In Windows, there is no centralized package management, so it is more difficult to have scripted installs. Another large aspect of a package manager is that they should handle software dependances. This means if some software requires another package, the package manager can get all of the required software and install it along with the requested package.

Along with package management, software configuration management is often more self contained in Linux when compared to Windows. There is no shared configuration location in Linux, like the Windows Registry. This means that each package has its own configuration location. This is often easier from a system administrator stand point.

## Kernel and Drivers

The kernel of a system is its base operating code. This is what connects the hardware of the system to the software that we use. Linus Torvalds argues that Linux itself is just the kernel and none of the attached software.  