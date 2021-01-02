install:
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y wireguard
	cp templates/wg0.conf /usr/local/etc/wireguard/
	cp templates/ios.conf /usr/local/etc/wireguard/
	cd /usr/local/etc/wireguard && umask 077
	wg genkey > /usr/local/etc/wireguard/freebsd.private
	wg pubkey < /usr/local/etc/wireguard/freebsd.private > /usr/local/etc/wireguard/freebsd.public
	wg genkey > /usr/local/etc/wireguard/ios.private
	wg pubkey < /usr/local/etc/wireguard/ios.private > /usr/local/etc/wireguard/ios.public
	sed -i "" "s~freebsd_private_key~`cat /usr/local/etc/wireguard/freebsd.private`~g" /usr/local/etc/wireguard/wg0.conf
	sed -i "" "s~ios_public_key~`cat /usr/local/etc/wireguard/ios.public`~g" /usr/local/etc/wireguard/wg0.conf
	sed -i "" "s~ios_private_key~`cat /usr/local/etc/wireguard/ios.private`~g" /usr/local/etc/wireguard/ios.conf
	sed -i "" "s~freebsd_public_key~`cat /usr/local/etc/wireguard/freebsd.public`~g" /usr/local/etc/wireguard/ios.conf
	sysrc gateway_enable="YES"
	sysrc wireguard_enable="YES"
	sysrc wireguard_interfaces="wg0"
	service wireguard start

deinstall:
	service wireguard stop
	sysrc -x wireguard_interfaces="wg0"
	sysrc -x wireguard_enable="YES"
	pkg del -y wireguard
