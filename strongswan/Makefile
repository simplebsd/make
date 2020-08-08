install:
	# Update pkg repo
	env ASSUME_ALWAYS_YES=YES pkg update

	# Install Strongswan
	pkg ins -y strongswan

	# Create certs directory
	mkdir -p /usr/local/etc/vpn-certs

	# Generate certs and keys
	ipsec pki --gen --type rsa --size 4096 --outform pem > /usr/local/etc/vpn-certs/server-root-key.pem
	chmod 600 /usr/local/etc/vpn-certs/server-root-key.pem
	ipsec pki --self --ca --lifetime 3650 --in /usr/local/etc/vpn-certs/server-root-key.pem --type rsa --dn "C=RU, O=SIMPLEBSD, CN=VPN Server Root CA" --outform pem > /usr/local/etc/vpn-certs/server-root-ca.pem
	ipsec pki --gen --type rsa --size 4096 --outform pem > /usr/local/etc/vpn-certs/vpn-server-key.pem
	chmod 600 /usr/local/etc/vpn-certs/vpn-server-key.pem
	ipsec pki --pub --in /usr/local/etc/vpn-certs/vpn-server-key.pem --type rsa | ipsec pki --issue --lifetime 1825 --cacert /usr/local/etc/vpn-certs/server-root-ca.pem --cakey /usr/local/etc/vpn-certs/server-root-key.pem --dn "C=RU, O=SIMPLEBSD, CN=simplebsd.ru" --san simplebsd.ru --flag serverAuth --flag ikeIntermediate --outform pem > /usr/local/etc/vpn-certs/vpn-server-cert.pem

	# Copy certs and keys to ipsec.d dirs
	cp /usr/local/etc/vpn-certs/vpn-server-cert.pem /usr/local/etc/ipsec.d/certs/
	cp /usr/local/etc/vpn-certs/vpn-server-key.pem /usr/local/etc/ipsec.d/private/

	# Add ipsec.conf file
	cp ipsec.conf /usr/local/etc/

	# Add ipsec.secrets file
	cp ipsec.secrets /usr/local/etc/

	# Add charon-logging.conf file
	cp charon-logging.conf /usr/local/etc/strongswan.d/charon-logging.conf

	# Create Strongswan log file
	touch /var/log/charon.log
	chmod a+w /var/log/charon.log

	# Enable and start Strongswan
	sysrc strongswan_enable="YES"
	service strongswan start

pf_configure:
	# Open 500, 4500 udp ports
	echo >> /etc/pf.conf
	echo '# Strongswan' >> /etc/pf.conf
	echo 'pass in on $$ext_if proto udp from any to $$ext_if port 500' >> /etc/pf.conf
	echo 'pass in on $$ext_if proto udp from any to $$ext_if port 4500' >> /etc/pf.conf
	pfctl -f /etc/pf.conf

pf_deconfigure:
	# Close 500, 4500 udp ports
	sed -i '' '/ext_if port 4500/d' /etc/pf.conf
	sed -i '' '/ext_if port 500/d' /etc/pf.conf
	sed -i '' '/Strongswan/d' /etc/pf.conf
	pfctl -f /etc/pf.conf

deinstall:
	# Disable and stop Strongswan
	service strongswan stop
	sysrc -x strongswan_enable

	# Uninstall Strongswan
	pkg del -y strongswan

	# Remove conf and log files
	rm -rf /usr/local/etc/strongswan*
	rm -rf /usr/local/etc/ipsec*
	rm -rf /usr/local/etc/vpn-certs
	rm -f /var/log/charon.log
