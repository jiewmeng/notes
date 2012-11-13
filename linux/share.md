Sharing
=======

	sudo cp /etc/samba/smb.conf.default /etc/samba/smb.conf
	sudo systemctl start smbd.service nmbd.service

	# in smb.conf
	[Public Share]
	path = /path/to/public/share
	available = yes
	browsable = yes
	public = yes
	writable = no