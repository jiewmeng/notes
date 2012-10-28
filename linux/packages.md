Packages
--------

### From `pacman`

	dbus xorg-server xorg-server-utils alsa-utils xf86-video-ati sudo xfce4 lxdm

`xf86-video-nouveau` for NVIDIA. 
`virtualbox-archlinux-additions` for Virtual Box guest

	xfce4-power-manager acpi pm-utils cpupower gedit chromium firefox p7zip file-roller ristretto git blender vlc flashplugin evince virtualbox pidgin gparted ntfs-3g meld xournal nodejs mongodb transmission-gtk gvfs gvfs-smb thunar-archive-plugin xfce4-screenshooter xfce4-taskmanager xfce4-mount-plugin mcomix libcups wget gummi youtube-dl openssh

### From AUR

First install `aurget`

	wget http://aur.archlinux.org/packages/au/aurget/aurget.tar.gz && tar -xvf ./aurget.tar.gz && cd aurget && makepkg -si

Then ... 

	sublime-text xfce-theme-greybird faenza-icon-theme profile-sync-daemon dropbox thunar-dropbox 

### From `npm -g`

	coffee-script express stylus mocha

### For Sublime Text 2

Package Control

	import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

Then ... 

- CodeIntel
- CoffeeScript
- Stylus
- Jade
