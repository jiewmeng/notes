Configuration
=============

### OpenBox

### `sudo`

	EDITOR=subl visudo

Then uncomment 

	%wheel ALL=(ALL) ALL

### Users

	adduser

##### Groups

	audio, lp, optical, storage, video, wheel, games, power, scanner

### Daemons 

- Manual: `rc.d start {moduleName}`
- Auto (`/etc/rc.conf`): `DAEMONS=({module}, ...)`
	- prefix with `@` to indicate start "async"

### Modules

- Manual: `modprobe -a {module} {module} ...`
- Auto:
	- Add `{module}.conf` to `/etc/modules-load.d/`
	- Contents will be just `{module-name}`

### Time

##### Hardware Clock

	hwclock -w --localtime

##### Timezone

	ln -sf /usr/share/zoneinfo/Asia/Singapore /etc/localtime

##### Timeformat

[more info](http://www.linuxquestions.org/questions/slackware-14/xfce-clock-custom-formatting-codes-753433/)

	%l:%M %p, %_d %b

### Keyboard Shortcuts

	/usr/bin/amixer set Master toggle
	/usr/bin/amixer set Master playback 5%+
	/usr/bin/amixer set Master playback 5%-
	/usr/bin/terminal

### Speed up thunar launch

Edit `/usr/share/gvfs/mounts/network.mount`

Change `automount=true` to `automount=false`