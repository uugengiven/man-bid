Installing systems used to be a one off type of event. Previously, a system would have Windows or Linux installed on it, then the OS would be updated and tweaked over the lifetime of the server to keep it running.

Today many systems run as what is called a stateless machine. This means that instead of installing once and slowly updating, each time there is a change to the systems that run on the server, the entire server is reinstalled from scratch.

Running stateless style servers makes a System Administrator's life much easier. Knowing that there are no special settings (or if there are, knowing that they are written down) in a system means it is much easier to debug. It also means that if hardware fails, it is much easier for a Sys Admin to restore whatever systems were running on that hardware.

A very important aspect of stateless servers is to make sure the server can be rebuilt exactly the same way, over and over again. This is normally done with scripts, so a machine can be built with no user intervention at all. When a new web host is needed, a script is run and when the script is done, the host is configured exactly like every other host in the environment.

This means that System Administrators must be much more comfortable with installing from a command line than they were in the past. It used to be very easy to follow the Microsoft Installation Wizard prompts to configure Windows Server. And, because they are often questions on the MCSA tests, it is a good idea to study and remember what those wizard options are. However, to be an effective System Administrator, it is very important to learn how to build systems via scripts.

We will first go through an install of windows with the Wizards to see what is happening, then we will go through the same install using a script.

## Determine Windows Install Requirments ##

### Edition ###

The edition of Windows Server to be installed is based on what license is needed. There are many versions of Server, although the most common are Datacenter, Standard, and Hyper-V. The Datacenter and Standard editions include extra OSE (operating system environments) which are generally used via Hyper-V or containers in some way.

Further information on the editions, including the extra editions are in 70-740 Exam Ref book, Chapter 1. While in the real world, Microsoft often provides companies with a licensing expert to help with these questions, the MCSA exam will often ask about editions and licensing. Covering this section carefully when prepping for an MCSA is highly recommended.

### Installation Type ###

Most editions of Windows Server let you chose between a GUI (graphical user interface) version of Windows called the Desktop Experience or a command line only version called Server Core. Historically, Windows has been written for the GUI first and then added command line support as a secondary option. This is less true now but often the default for most Windows System Administrators is to use the Desktop Experience. Throughout this course we will do both types of install.

### Roles and Features ###

As we covered when working with DNS and File Shares, Roles and Features are what makes a Windows Server install useful. Different Roles or Features can add additional server requirements, such as extra network connections, more RAM, more processors, or more. 

### Virtualization ###

A final consideration when installing Windows onto physical hardware is whether this hardware should be a host for virtual machines or not. The Hyper-V role is something we have already set up for a lot of the class and we will go use it regularly before installing and configuring it. However, at this point, almost no system should be installed onto base physical hardware. These systems are very hard to update or move due to changes in the physical hardware and depending on the Roles and Features they're running, they may have no way to provide High Availability for the workloads they are running.

Unless there is a good business reason to change, the default for all servers running business software is for them to be run as virtual machines.

The installation of a VM host is a separate topic from installing a Windows server with a different Role or Feature. 

## Installing Windows Via GUI ##

> Have students walk through the installation of Windows via the wizard. Have students install both the Desktop Edition and the Server Core version. 

> Once the machines are installed, have students set a static IP, install the File Shares Role and share a folder to the network with anonymous read/write. They should do this on both installation types, core and desktop, using the GUI in desktop to do setup and Powershell to setup the core environment.

> Have students take their install and write a script that will set the IP and enable the File Shares Role and setup the share folder, then reinstall both versions of Windows and run the script to show that the script will run on either version of windows.
> These should all be Powershell commands that the students used in week 2.

> * The reinstall is not required, and showing students to take VM snapshots and reverting back to a snapshot could also be used here.

## Updates after Install ##

Microsoft does not release a new image for Windows every time there is a Windows Update. Images are released on a semi-regular schedule and it is up to System Administrators to update images with newer updates, drivers, and hotfixes as needed.

If a Sys Admin doesn't update their images, the first thing a new server will have to do is immediately run Windows Updates to be patched. This can a) take a lot of time and b) makes it harder for systems to be stateless.

So, how do we update an image to include newer updates, or even custom software?

### DISM ###

*Deployment Image Services and Management* or **DISM** is the tool provided by Microsoft to update Windows Imaging files (.wim) so they can be more useful to a System Administrator. The Admin can add Windows Updates, Pre-Install Roles or Features, add Custom Software, and more to the image using DISM.

DISM allows you to mount an image file, push updates to that image, then save the changes to the original image file. The mounting is done with the following command:

`dism /mount-image /imagefile:[filename] /index:[index# of image] /mountdir:[pathname]`

Once mounted, the rest of the DISM commands can be used on the image to update the image as needed. One example of this is adding a Windows Update.

`dism /image:[pathname] /add-package /packagepath:[path to updates]`

When all of the changes are done on the image, it needs to be unmounted and the changes either committed or thrown away. This is done with the following command:

`dism /unmount-image /mountdir:[pathname] [/commit or /discard]`

This updated image can now be used to install Windows with appropriate update levels, roles, and any other customizations that DISM can provide.

> Have students update the image file they have been using, adding a windows update and adding a folder into the image that holds a useful exe (perhaps Notepad++), then have them install Windows with that image and see how those changes were applied.

## The Install Wizard ##

We have seen how to install and update images in Windows. How do we now make the full install automated? There are several options, but the one we will use in a future week is Windows Deployment Services. This will allow us to set up full configuration and install scripts for windows and make sure every aspect is scripted out and no wizard is needed at all.