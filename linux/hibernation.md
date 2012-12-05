Hibernation
===========

[Arch Docs Link](https://wiki.archlinux.org/index.php/Pm-utils)

Add `resume=/dev/sdXX` where `/dev/sdXX` is swap partition. 
On syslinux, it looks like 

    APPEND resume=/dev/sda5 ... 

Then if system doesn't resume ... edit `/etc/mkinitcpio.conf`. 
Add `resume` too `HOOKS` array, between `ide | scsi | sata | lvm2`
and `filesystems`. 

	HOOKS="... sata resume filesystems ..."

The rebuild `initrd` image: 

	mkinitcpio -p linux