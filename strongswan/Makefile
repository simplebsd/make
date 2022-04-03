install:
	@echo
	@echo [Update pkg repo]
	@echo
	env ASSUME_ALWAYS_YES=YES pkg update
	@echo

	@echo [Install Strongswan]
	@echo
	pkg ins -y strongswan
	@echo

	@echo [Create certs directory]
	@echo
	mkdir -p /usr/local/etc/vpn-certs
	@echo

	@echo [Generate certs and keys]
	@echo
	ipsec pki --gen --type rsa --size 4096 --outform pem > /usr/local/etc/vpn-certs/server-root-key.pem
	chmod 600 /usr/local/etc/vpn-certs/server-root-key.pem
	ipsec pki --self --ca --lifetime 3650 --in /usr/local/etc/vpn-certs/server-root-key.pem --type rsa --dn "C=RU, O=SIMPLEBSD, CN=VPN Server Root CA" --outform pem > /usr/local/etc/vpn-certs/server-root-ca.pem
	ipsec pki --gen --type rsa --size 4096 --outform pem > /usr/local/etc/vpn-certs/vpn-server-key.pem
	chmod 600 /usr/local/etc/vpn-certs/vpn-server-key.pem
	ipsec pki --pub --in /usr/local/etc/vpn-certs/vpn-server-key.pem --type rsa | ipsec pki --issue --lifetime 1825 --cacert /usr/local/etc/vpn-certs/server-root-ca.pem --cakey /usr/local/etc/vpn-certs/server-root-key.pem --dn "C=RU, O=SIMPLEBSD, CN=193.109.79.126" --san 193.109.79.126 --flag serverAuth --flag ikeIntermediate --outform pem > /usr/local/etc/vpn-certs/vpn-server-cert.pem
	@echo

	@echo [Copy certs and keys to ipsec.d dirs]
	@echo
	cp /usr/local/etc/vpn-certs/vpn-server-cert.pem /usr/local/etc/ipsec.d/certs/
	cp /usr/local/etc/vpn-certs/vpn-server-key.pem /usr/local/etc/ipsec.d/private/
	@echo

	@echo [Add ipsec.conf file]
	@echo
	cp ipsec.conf /usr/local/etc/
	@echo

	@echo [Add ipsec.secrets file]
	@echo
	cp ipsec.secrets /usr/local/etc/
	@echo

	@echo [Add charon-logging.conf file]
	@echo
	cp charon-logging.conf /usr/local/etc/strongswan.d/charon-logging.conf
	@echo

	@echo [Create Strongswan log file]
	@echo
	touch /var/log/charon.log
	chmod a+w /var/log/charon.log
	@echo

	@echo [Enable and start Strongswan]
	@echo
	sysrc strongswan_enable="YES"
	service strongswan start

pf_configure:
	@echo [Open 500, 4500 udp ports]
	@echo
	echo >> /etc/pf.conf
	echo '# Strongswan' >> /etc/pf.conf
	echo 'pass in on $$ext_if proto udp from any to $$ext_if port 500' >> /etc/pf.conf
	echo 'pass in on $$ext_if proto udp from any to $$ext_if port 4500' >> /etc/pf.conf
	pfctl -f /etc/pf.conf

pf_deconfigure:
	@echo [Close 500, 4500 udp ports]
	@echo
	sed -i '' '/ext_if port 4500/d' /etc/pf.conf
	sed -i '' '/ext_if port 500/d' /etc/pf.conf
	sed -i '' '/Strongswan/d' /etc/pf.conf
	pfctl -f /etc/pf.conf

deinstall:
	@echo [Disable and stop Strongswan]
	@echo
	service strongswan stop
	sysrc -x strongswan_enable
	@echo

	@echo [Uninstall Strongswan]
	@echo
	pkg del -y strongswan
	@echo

	@echo [Remove conf and log files]
	@echo
	rm -rf /usr/local/etc/strongswan*
	rm -rf /usr/local/etc/ipsec*
	rm -rf /usr/local/etc/vpn-certs
	rm -f /var/log/charon.log
