## Final projects

### Windows - 1 week
Set up a 2-site domain
* an internal website with SQL (use IIS)
* Exchange
  * with a relay for a secondary domain
	* allow web and mobile connections (ActiveSync)
* WSUS
* Desktop deployment via WDS
* Use GPOs for configuration
* Powershell setup script for installing common programs
* make backup plan using Hallengren scrips

Tests for completeness:
* PXE boot to install OS
* Login with domain user
* send/receive email (with mobile)
* verify that website loads
* verify GPO settings

### Linux - 2 weeks
Set up OpenStack
* Use it to host the Windows stuff
* Local cache based on NGINX
* Basic Postgres setup (Gitlab)
  * Question for research: Does Openstack use SDN?
* LAMP stack - install and host basic website (Wordpress)

## Topics to cover in class

John's list:
* DNS - use and setup
* IP networking/network setups
  * CIDR notation
	* firewalls
	* ports
	* forwarding
	* VPNs
* Active Directory
* GPO (Group Policy)
* reading logs - Windows and Linux
* setting up your environment
  * bginfo (Windows)
	* dot scripts. editor. (Linux)
		* vi and emacs - survival mode
* shells/command prompt
* programming shell scripts
* DBs
	* SQL, Microsoft SQL
	* Postgres/MySQL
* Mail servers
* Web hosting

Guest speaker topics:
* git
* how the internet works
* Linux flavors
* certifications
* troubleshooting storytime
* social engineering

Jean's additions (items that overlay everything):
* Meet your neighbors
* security

## Schedule
24 total weeks.
* 16 weeks of learning (see below)
* 4 of final projects/job development/interviewing
* 4 of externship

### Week 1
* Intro to systems
* Intro to networking
* Basic permissions
* Intro to command line

### Week 2
* Networking and DNS deeper dive
* Setup and use of Microsoft DNS
* Fileshares
* Firewall basics

### Week 3
* Windows domain
* Group policy
* Active Directory and AD user administration
* IIS hosting
* Intro to web hosting

### Week 4
* Intro to DBs
  * SQL
  * Microsoft MySQL
* Backups (Hallengren scripts)

### Week 5
* Intro to mailing
  * Exchange
  * backup
	* Activesync
* Nebulous WSUS something

### Week 6
* Troubleshooting/monitoring in Windows
  * Windows logging
	* Event viewer
	* REPL admin
	* Powershell stuff
* Network troubleshooting
* Setting up your Windows environment

### Week 7
* Installing and configuring things in Linux
* vi/emacs crash course
* Users in default Linux
* "Not everything needs 777"

### Week 8
* Apache/NGINX - webhosting in Linux
* Postgres OR MySQL - DBs in Linux
* Linux DNS

### Week 9
* SendMail
* Other DB - whichever one didn't happen in week 8
* Troubleshooting/monitoring in Linux

### Week 10
* Intro to containers
* Docker
* Managing containers
* Desired configuration

### Week 11
* Building containers
* Sharing containers

* Shell scripting goes somewhere in here?!

### Week 12
* Virtualization
  * shared storage/ISCSI
	* HyperV/VMWare (whichever makes sense)
	* Images/cloning
	* Virtual networks/SDN

### Week 13
* VM management
* VM host design - hypervisors

### Week 14
* Road to OpenStack (RESEARCH NEEDED)

### Week 15
* Build out Azure with design toward OpenStack

### Week 16
* OpenStack deep dive