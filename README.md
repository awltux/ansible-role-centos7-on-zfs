# fedora-hosted-docker-cluster-ansible-playbook
Ansible playbook that manages a Fedora hosted Docker cluster

## Setup a Fedora-server:28 instance

Create an 'admin' user, add to 'wheel' group.
Configure sudo to allow NO_PASWD on wheel group.   

This server is the first member of the Docker cluster.
It will eventually be re-imaged once cluster is properly initialised.

```
git clone https://github.com/awltux/fedora-hosted-docker-cluster-ansible-playbook.git
cd fedora-hosted-docker-cluster-ansible-playbook/ansible
ansible-playbook setup-bootstrap-host.yml
```

# VirtualBox Test Machine
If you want to experiment this playbook, create a VirtualBox machine as follows:
* Download the Fedora28 network iso
* Create a new Machine
** Memory 4G
** Disk 70G (or larger of you want to do anything useful with it)

## Machine Settings (before turning on)
* System | Motherboard | Enable EFI = selected
* Storage | Storage Devices | Controller IDE -> Empty = Selected
** Attributes | Optical Drive | Optical Disk icon = Select Fedora iso form file system
* Audio | Enable Audio = de-selected
* Network | Adapter 1 | Advanced | Port Forwarding
** 'Add New Port Forwarding rule' icon
*** name=ssh; HostPort=2222; GuestPort=22
** OK
* OK

## Start Machine and follow fedora install
INSTALLATION DESTINATION
* Storage Configuration | Advanced Custom | Done
* Edit Selected Device (cogs icon) | Set Partition Table | gpt = selected
* Add New Device (Plus icon)
** Size: 200Mb
**  File System

## Create Boot Partitioning
* Size: 200Mb
* FileSystem: EFI System Partition
* MountPoint: /boot/efi
* OK
	
## Create empty partition for zpool
* Size: ~ DiskSize - 30GB = 70 - 30 = 40GB
* FileSystem: unformatted
* OK
 
## Create partition for Fedora Install
* Size: remaining space ~30GB
* FileSystem: ext4
* MountPoint: /
* OK
* Done ( x2 to ignore message about missing swap; we create a zfs based swap later on)

## NETWORK & HOSTNAME
Ensure network is on and connected

## Begin Installation
Create an 'admin' user, with admin rights; server hardening will disable remote access for root


# Setup SSH keys
Either:
* Use scp to copy pre-existing ~admin/.ssh folder onto home dir 
* Ensure that the id_rsa private key is set to 600 permissions 

OR:
* Create new 4096 bit rsa keys
    ```ssh-keygen -t rsa -b 4096```



Now add this key to the authorized folder
    ```ssh-copy-id admin@localhost```
Copy these files to a secure area; so next-time you can use the alternative method above

# Putty Auto-Login
If using Putty, import the private id_rsa key and then save it as id_rsa.ppk
Use id_rsa.ppk in putty connections to auto login 

Modify /etc/sudoers file to allow auto-login
    ```sudo sed -i "s/# \(%wheel.*NOPASSWD:\)/\1/" /etc/sudoers```
	
Install basic tools for getting and running playbook
    ```sudo yum install -y git ansible```

Clone the ansible playbook
    ```git clone https://github.com/awltux/fedora-hosted-docker-cluster-ansible-playbook.git```

# Set some aliases to help developer
```
cat >> ~admin/.bashrc <<HEREDOC
alias vi-task="cd ~admin/fedora-hosted-docker-cluster-ansible-playbook/ansible; vi roles/zfs-setup/tasks/main.yml"
alias vi-pb="cd ~admin/fedora-hosted-docker-cluster-ansible-playbook/ansible; vi setup-bootstrap-host.yml"
alias run-pb="cd ~admin/fedora-hosted-docker-cluster-ansible-playbook/ansible; ansible-playbook setup-bootstrap-host.yml"
HEREDOC
source ~admin/.bashrc
	
cd fedora-hosted-docker-cluster-ansible-playbook/ansible
```
