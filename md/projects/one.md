### Linux (2 Weeks):

This project is to create an OpenStack environment using one of the Hypervisors listed: VMWare, HyperV, KVM

This project should follow the basic OpenStack setup as seen here: http://docs.openstack.org/newton/install-guide-ubuntu/

After creating a fully functioning OpenStack environment that allows for multi-tenant hosting, the next steps are to create three hosted projects:

 * Webcache via nginx
  * This webcache can be for any site or service
  * For this to work, dns will have to be rewritten (www.imgur.com points to the local nginx cache, then it breaks out to the real imgur)
 * Wordpress site with a "hello world" type page
 * Gitlab (http://www.gitlab.com) installation that allows users to log in and store code


 ### Tests for Completion

 * OpenStack administration site loads and two separate users exist with separate machines/domains/spaces
  * This is the main test for completeness
 * Gitlab site loads and allows user to log in
 * Wordpress site loads
 * Downloads that go through the webcache are faster, logs show downloads happening via webcache
