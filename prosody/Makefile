install:
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y prosody
	openssl req -new -x509 -days 365 -nodes -subj "/C=RU/ST=Moscow/L=Moscow/O=IT/CN=example.com" -out "$(DM).crt" -newkey rsa:2048 -keyout "$(DM).key"
	cp templates/prosody.cfg.lua /usr/local/etc/prosody/
	sed -i "" "s/example.com/$(DM)/g" /usr/local/etc/prosody/prosody.cfg.lua
	sysrc prosody_enable="YES"

deinstall:
	service prosody stop
	sysrc -x prosody_enable="YES"
	pkg del -y prosody
