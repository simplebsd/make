install:
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y prosody
	openssl req -new -x509 -days 365 -nodes -subj "/C=RU/ST=Moscow/L=Moscow/O=IT/CN=$(DM)" -out "/usr/local/etc/prosody/$(DM).crt" -newkey rsa:2048 -keyout "/usr/local/etc/prosody/$(DM).key"
	cp templates/prosody.cfg.lua /usr/local/etc/prosody/
	sed -i "" "s/example.com/$(DM)/g" /usr/local/etc/prosody/prosody.cfg.lua
	chown -R prosody:prosody /usr/local/etc/prosody
	sysrc prosody_enable="YES"
	service prosody start

deinstall:
	service prosody stop || true
	sysrc -x prosody_enable="YES"
	pkg del -y prosody || true
	rm -rf /usr/local/etc/prosody
