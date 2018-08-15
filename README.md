# Ansible-Dropbox with directory exclude (selective sync)

### That's what the role does:
* Installing Dropbox directly on a CentOS machine.
* Creates a userspecific systemd.service file. Multiple role runs for multiple users should be possible.
* Starts the dropbox daemon with user rights and checks whether the host is already linked.
* If not linked, it prints the URI to click and waits depending on operation mode (see below).
* Enables selective sync. Excluding directories is possible.

### The problem:
* Installing Dropbox on a Linux machine requires to link the device. Linking the device is done by clicking on a URL and logging in to Dropbox. That kinda kills a fully automated install.

### The solution:
* The role supports two ways of operation. These are controlled by the variable ```dropbox_unattended```.
* When ```dropbox_unattended: true```, then
  * the role does not stop and runs through.
  * If the device has not been linked yet, the URL to link the device is printed.
  * Obviously no directory exclude will be configured when the device is not yet linked.
  * Simply run the role again after linking and the direcotries will be excluded.
  * This mode is used for Travis CI testing.
* When ```dropbox_unattended: false```, then
  * the role pauses and waits for unser interaction.
  * This is the default.
  * You get prompted when you have to click the link.
  * After linking, press enter in the Ansible run.

## Usage

TODO After linking to Ansible Galaxy

Configure your variables, see ```default``` folder for example.