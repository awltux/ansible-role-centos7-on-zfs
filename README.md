# ansible-playbook-centos7-on-zfs
# WARNING: STILL WIP

There are a number of examples on how CentOS 7 can be converted to a ZFS root FS, but there don't seem to be any scripted examples.

This ansible role is an attempt to convert a standard (xfs, ext3, ext4) CentOS 7 system to one based on ZFS. 

Target features:
* Support both UEFI and BIOS booting
* Auto-rebuild kernel modules if kernel updated
* Script to update grub menu to provide entries for specific zfs snapshots of the system
