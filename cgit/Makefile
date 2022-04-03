install:
	@echo
	@echo [Update repo and install Nginx, Fcgiwrap, Git and Cgit]
	@echo
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y nginx fcgiwrap git cgit
	@echo

	@echo [Add Git group and user]
	@echo
	pw groupadd -n git || true
	pw useradd -n git -c "Git user" -s /usr/local/libexec/git-core/git-shell -d /git || true
	@echo

	@echo [Configure Nginx]
	@echo
	cp -f templates/nginx.conf /usr/local/etc/nginx/
	@echo

	@echo [Enable and start Nginx, Fcgiwrap]
	@echo
	sysrc nginx_enable="YES"
	sysrc fcgiwrap_enable="YES"
	sysrc fcgiwrap_user="www"
	sysrc fcgiwrap_socket_owner="www"
	sysrc fcgiwrap_socket_group="www"
	service nginx start
	service fcgiwrap start

deinstall:
	@echo
	@echo [Stop Nginx, Fcgiwrap service and disable it]
	@echo
	service fcgiwrap stop
	service nginx stop
	sysrc -x nginx_enable="YES"
	sysrc -x fcgiwrap_enable="YES"
	sysrc -x fcgiwrap_user="www"
	sysrc -x fcgiwrap_socket_owner="www"
	sysrc -x fcgiwrap_socket_group="www"
	@echo

	@echo
	@echo [Uninstall Nginx, Fcgiwrap, Git and Cgit]
	pkg del -y nginx fcgiwrap git cgit
