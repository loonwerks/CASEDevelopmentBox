# Create new OSATE2 development tarball

Presently, there is no path simple path towards automating the
installation of the OSATE2 development environments. Specifically, the
installation dependencies are maintained in a setup XML specification
that the OOMPH installation infrastructure can interpret but this
infrastructure has no mechanism for headless operation. As a work
around, the OSATE2 development environment is installed manually on an
overlay file system that a tarball is created from. During
provisioning this tarball can be untarred. The benefit of this
approach is that it permits a way to automate the construction of a
CASE development environment. The weakness of this approach is that
when the setup specification changes, something that hopefully will
happen infrequently, a new tarball will need to be created.

The following steps describe how to create a new tarball:

    1. Comment out the line in the user-land provisioning segment
    within the Vagrantfile that untars the osate2 tarball.
    2. Provision a new box with "vagrant up". If the box is already up
    it can be torn down with "vagrant destroy". Waiting for the
    provisioning to complete before continuing to the next step.
    3. Log into the box via "vagrant ssh".
    4. Make the directory
- mkdir /tmp/upper
- mkdir /tmp/workdir
- sudo mount -t overlay -o lowerdir=/home/vagrant,upperdir=/tmp/upper,workdir=/tmp/workdir none /home/vagrant
- cd software/eclipse-install/
- ./eclipse-install
- click on hamburger menu at top-right corner of menu and choose "advanced mode"
- highlight "Eclipse Modelling Tools" and modify "Bundle Pool" path to
  "/home/vagrant/dev/osate2/.p2/pool" then click next
- Highlight "Github Projects" then click on plus button in the top-right corner
- Enter "https://raw.githubusercontent.com/osate/osate2/develop/setup/osate2.setup" in the "Resource URIs" text box.
- Select the "OSATE2 Development" item and click next.
- Enter "/home/vagrant/dev/osate2" in the "Root install folder:" text box.
- Enter "/home/vagrant/repos" in the "Git clone location:" text box.
- Click on the arrows to the right of the "OSATE2", "SMACCM", and
"Ocarina" text boxes and select "HTTPS (read-only, anonymous)" in each arrow.
- Click "Next".
- Click "Finish".
- Click through multiple license agreements shown during installation process.
- At some point, Eclipse will start and the "Finish" button can be
  clicked on the installation dialog.
- Eclipse should now be "Executing startup tasks"; do not move onto
the next step until these tasks are complete.
- Close eclipse.
- Run tar zcvf /vagrant/osate2-develop-overlay-${DATE}.tar.gz -C /tmp/upper/ ./
- sudo umount /home/vagrant
- Exit the vagrant virtual machine.
- vagrant destory
- Modify the filename to be untarred in the Vagrantfile
- Uncomment out the line that untars the osate2 tarball in the vagrantfile
- vagrant up
