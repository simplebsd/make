install:
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y postfix dovecot
	cp -r /usr/local/etc/dovecot/example-config/* /usr/local/etc/dovecot/
	sysrc postfix_enable="YES"
	sysrc dovecot_enable="YES"
	

deinstall:
	pkg del -y postfix dovecot
	sysrc -x postfix_enable
	sysrc -x dovecot_enable
