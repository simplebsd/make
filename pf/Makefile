install:
	# Add pf.conf file
	cp pf.conf /etc/

	# Enable and start PF
	sysrc pf_enable="YES"
	sysrc pf_rules="/etc/pf.conf"
	service pf start
