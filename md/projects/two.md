## Windows (2 Weeks):

In this final project, students will be using the OpenStack environment from the Linux final to set up a full Windows domain. This domain replicates a multi-site office setup with several services, such as Exchange, IIS, WSUS, and WDS for desktop OS deployment. Along with these services, GPOs will be used to automatically configure certain values on user computers.

The students should set up the following, with appropriate networking as needed:
 * Setup a 2 site domain - each in their own subnets (10.0.1.0/24 and 10.0.2.0/24 possibly)
 * Setup Desktop deployment in the domain using WDS
 * Use GPOs for user/desktop configuration, including default web page, default screen saver
 * Create an Exchange server for mail hosting, including activesync
 * Install an IIS website that requires SQL (install SQL as well)
  * Website to be provided as a ZIP or other format to deployment
  * Database to be provided as a .bak to be restored
  * Website web.config should require the proper dns for the database to be entered
 * Create a backup plan for the environment, including SQL, Exchange, IIS, and other servers. No actual backup functionality needs to be built unless students have time

### Test functionality

 * PXE Boot to install OS
  * A new VM should be provisioned and an, using PXE, Windows should be installed with the machine being automatically added to the domain
  * VM should have pre-installed software from deployment: notepad++ or putty are good ones to test
 * Login to new machine with domain user
 * Send/receive email with mobile device or something replicating a mobile device (or Outlook verifying the autodiscover)
  * This may only work on the local network, that is fine.
 * Verify that website loads
 * Verify GPO settings (IE branding, background picture, screen saver)
 * Verify that domain replication is happening between sites
