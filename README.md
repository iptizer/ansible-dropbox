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

1. Install role

```bash
cd yourPlaybookFolder/
ansible-galaxy install iptizer.ansible_dropbox
```

2. Create Playbook

```bash
vim dropbox.yml
## something like
- hosts: all
  roles:
    - iptizer.ansible_dropbox
```

3. Configure your Variables, or leave default and run playbook.

```bash
ansible-plabook dropbox.yml -l <<your destination host>>
```

4. Due to default setting unattended=false, playbook will stop and prompt a URL. Click it and press ENTER.

That's it.