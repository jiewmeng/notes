# Arch Linux Install

## 1. Partitioning/Formatting

Easier to just use GParted via [Parted Magic][pmagic] to partition and format. Otherwise use
`fdisk`, `gdisk`, `parted` etc.

	/boot	SSD	200MB	ext2
	/	SSD	42GB	ext4
	/win	SSD	~86GB	ntfs
	/var 	HDD	12GB	ReiserFS ([good for small files][fs-for-small-files])
	swap	HDD	16GB	swap

[fs-for-small-files]: https://wiki.archlinux.org/index.php/File_Systems#Type_of_File_Systems
[pmagic]: http://partedmagic.com/

### SSD Optimizations

#### TRIM/Discard

	/dev/sda1  /       ext4   defaults,noatime,discard   0  1

Note the use of `noatime` too

#### IO Scheduler

	echo noop > /sys/block/sdX/queue/scheduler

Or just edit file. Selected option is denoted by square brackets

#### Swappiness

Edit `/etc/sysctl.conf`

	vm.swappiness=1
	vm.vfs_cache_pressure=50

#### Browser Profiles

Install `profile-sync-daemon` from AUR. 

#### Increase TMPFS size

	tmpfs     /scratch     tmpfs     nodev,nosuid,size=7G     0     0

#### Writeback for ReiserFS (`/var`)

	data=writeback,notail

## 2. Mount & Install Base System

Mount partitions with `/` (root) on `/mnt`. Then others like `/boot` on `/mnt/boot`. For example:

	mount /dev/sda2 /mnt
	mkdir /mnt/{boot,var}
	mount /dev/sda1 /mnt/boot
	mount /dev/sdb3 /mnt/var

Then ... 

	pacstrap /mnt base base-devel syslinux

Alternative boot loaders: `grub-bios` or `grub-efi-x86_64`. NOTE: For `syslinux` on GPT disks, 
install `gptfdisk` first.

Configure `/boot/syslinux/syslinux.cfg`

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

Exit, unmount and reboot: `umount /mnt/{boot,var}`

## 3. User Management

	useradd -m -g [initial_group] -G [additional_groups] [username]

- `-m`: to add home directory
- `-G`: additional groups like `wheel,storage,audio,video,power,lp,scanner,optical,games`

Next `passwd [username]` to change the password

## 4. Packages

	mesa xorg-server xorg-server-utils alsa-utils xf86-video-ati sudo gnome

- `xf86-video-nouveau` for NVIDIA. 
- `virtualbox-archlinux-additions` for Virtual Box guest

	gedit chromium firefox p7zip file-roller eog git vlc evince gparted ntfs-3g samba gvfs gvfs-smb wget openssh libreoffice flashplugin blender meld xournal nodejs mongodb transmission-gtk gummi  youtube-dl gimp blender gnome-screenshot virtualbox virtualbox-guest-iso nautilus-open-terminal gnome-tweak-tool zeitgeist

### GitHub

Setup SSH Key `ssh-keygen -t rsa`

	git config --global user.email "{name@email.com}"
	git config --global user.name "{your name}"

### Virtual Box

Create a file (`/etc.modules-load.d/vbox.conf`) with contents 

	vboxdrv
	vboxnetadp
	vboxnetflt

Others?

	eclipse

#### Configure & Start Samba

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

### AUR

	wget http://aur.archlinux.org/packages/au/aurget/aurget.tar.gz && tar -xvf ./aurget.tar.gz && cd aurget && makepkg -si

	aurget -S sublime-text profile-sync-daemon dropbox nautilus-dropbox speedcrunch

#### Configure `profile-sync-daemon`

Edit `/etc/psd.conf`

	USERS="jiewmeng"

#### Sublime Text 

Install package control

	import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

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
