Thirst, in file Makefile in section # Generate certs and keys, change configuration for your server.

Next, set the server domain or ip address in file ipsec.conf

And then set username and password in file ipsec.secrets.

After this do:
make install

If you have enabled PF, you can open ports for Strongswan by command:
make pf_configure
WARNING: make sure you have PF conf file, located at /etc/pf.conf, and this conf file has external network interface variable called $ext_if.

To uninstall Strongswan, do:
make deinstall

To remove Strongswan PF configuration, do:
make pf_deconfigure
