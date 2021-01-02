install:
        env ASSUME_ALWAYS_YES=YES pkg update
        pkg ins -y wireguard
        cp templates/wg0.conf /usr/local/etc/wireguard/
        cd /usr/local/etc/wireguard && umask 077
        wg genkey > /usr/local/etc/wireguard/freebsd_client.private
        wg pubkey < /usr/local/etc/wireguard/freebsd_client.private > /usr/local/etc/wireguard/freebsd_client.public
        sed -i "" "s~freebsd_client_private_key~`cat /usr/local/etc/wireguard/freebsd_client.private`~g" /usr/local/etc/wireguard/wg0.conf
        sysrc wireguard_enable="YES"
        sysrc wireguard_interfaces="wg0"
#       service wireguard start

deinstall:
        service wireguard stop
        sysrc -x wireguard_interfaces="wg0"
        sysrc -x wireguard_enable="YES"
        pkg del -y wireguard
