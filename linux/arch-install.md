# Arch Linux Install

## 1. Partitioning/Formatting

Easier to just use GParted via [Parted Magic][pmagic] to partition and format. Otherwise use
`fdisk`, `gdisk`, `parted` etc.

## 2. Mount & Install Base System

Mount partitions with `/` (root) on `/mnt`. Then others like `/boot` on `/mnt/boot`. For example:

	mount /dev/sda1 /mnt

Then ... 

	pacstrap /mnt base base-devel syslinux

Alternative boot loaders: `grub-bios` or `grub-efi-x86_64`. NOTE: For `syslinux` on GPT disks, 
install `gptfdisk` first.

Generate `fstab` using

	genfstab -p /mnt >> /mnt/etc/fstab

Chroot into system 

	arch-chroot /mnt

Write hostname to `/etc/hostname`

Configure timezone

	ln -s /usr/share/zoneinfo/Asia/Singapore /etc/localtime

Edit `/etc/locale.conf`

Run `mkinitcpio -p linux`

Change password: `passwd`

Install syslinux using `syslinux-install_update.sh -i -a -m`

Configure `/boot/syslinux/syslinux.cfg`

Exit, unmount and reboot: `umount /mnt`

## 3. User Management

	useradd -m -g [initial_group] -G [additional_groups] [username]

- `-m`: to add home directory
- `-G`: additional groups like `wheel,storage,audio,video,power,lp,scanner,optical,games`

Next `passwd [username]` to change the password

## 4. Packages

	mesa xorg-server xorg-server-utils alsa-utils xf86-video-ati sudo xfce4

- `xf86-video-nouveau` for NVIDIA. 
- `virtualbox-guest-utils` for Virtual Box guest

	chromium firefox p7zip git vlc gparted ntfs-3g samba gvfs gvfs-smb wget openssh libreoffice flashplugin meld xournal nodejs mongodb transmission-gtk gummi texlive-most youtube-dl gimp blender virtualbox virtualbox-guest-iso pidgin skype gksu opendesktop-fonts file-roller ristretto xfce4-screenshooter evince

    	xfce-theme-greybird faenza-xfce-addon faience-icon-theme  faience-themes

### AUR with `yaourt`

Add to `/etc/pacman.conf`

	[archlinuxfr]
	SigLevel = Never
	Server = http://repo.archlinux.fr/$arch

Then install 

	pacman -Syu yaourt

Use it

	yaourt -S sublime-text profile-sync-daemon dropbox speedcrunch skype4pidgin dropbox-cli pidgin-opensteamworks-bin

### GitHub

Setup SSH Key `ssh-keygen -t rsa`

	git config --global user.email "{name@email.com}"
	git config --global user.name "{your name}"
	git config --global push.default simple

### Virtual Box

Create a file (`/etc/modules-load.d/vbox.conf`) with contents 

	vboxdrv
	vboxnetadp
	vboxnetflt

Others?

	eclipse

### Configure & Start Samba

	cp /etc/samba/smb.conf.default /etc/samba/smb.conf

Start service:

	sudo systemctl enable smbd.service nmbd.service

Add user shares like: 

	# in smb.conf
	[SHARE NAME]
	path = /path/to/share
	available = yes
	browsable = yes
	public = yes
	writable = no

#### Configure `profile-sync-daemon`

Edit `/etc/psd.conf`

	USERS="jiewmeng"

#### Sublime Text 

Install package control

	import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

## 5. SSD Optimizations

### TRIM/Discard

	/dev/sda1  /       ext4   defaults,noatime,discard   0  1

`discard` option in `fstab`. Note use of  `noatime` too

### IO Scheduler

	echo noop > /sys/block/sdX/queue/scheduler

Or just edit file. Selected option is denoted by square brackets

### Swappiness

Edit `/etc/sysctl.conf`

	vm.swappiness=1
	vm.vfs_cache_pressure=50

### Browser Profiles

Install `profile-sync-daemon` from AUR. 

# Windows Stuff (Timezone)

Make windows use UTC time

	Windows Registry Editor Version 5.00

	[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation]
	     "RealTimeIsUniversal"=dword:00000001

## 6. Locale stuff

Edit `/etc/locale.conf`

	LANG="en_US.UTF-8"

# Windows Stuff (Optimization)

See http://www.overclock.net/t/1156654/seans-windows-7-install-optimization-guide-for-ssds-hdds

- Disable Indexing, System Restore, Paging
- Disable hibernation (`powercfg -h off`)
- Update Drivers (see also http://www.intel.com/p/en_US/support/detect)
- Disable SuperFetch
	- Services.msc
	- Disable SuperFetch
- Disable Prefetch
	- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters
 	- set EnablePrefetcher = 0
	- set EnableSuperfetch = 0

[fs-for-small-files]: https://wiki.archlinux.org/index.php/File_Systems#Type_of_File_Systems
[pmagic]: http://partedmagic.com/
