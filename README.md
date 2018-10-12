# ansible-playbook-centos7-on-zfs

There are a number of examples on how CentOS 7 can be converted to a ZFS root FS, but there don't seem to be any scripted examples.

This ansible role is an attempt to convert a standard (xfs, ext, ext4) CentOS 7 system to one based on ZFS. 

WARNING: STILL WIP

Requirements:
* Support both UEFI and BIOS booting
