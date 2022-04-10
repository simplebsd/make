install:
	@echo
	@echo [Install Nginx, Php and Kanboard packages]
	@echo
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y nginx php80 php80-kanboard
	@echo

	@echo [Configure Kanboard]
	@echo
	cp templates/nginx.conf /usr/local/etc/nginx/
	chown -R www:www /usr/local/www/kanboard
	@echo

	@echo [Enable and start Nginx, Php services]
	@echo
	sysrc nginx_enable="YES"
	sysrc php_fpm_enable="YES"
	service nginx start
	service php-fpm start

deinstall:
	@echo
	@echo [Disable and stop Nginx, Php services]
	@echo
	service nginx stop || true
	service php-fpm stop
	sysrc -x nginx_enable
	sysrc -x php_fpm_enable
	@echo

	@echo [Uninstall Nginx, Php and Kanboard packages]
	@echo
	pkg del -y nginx php80 php80-kanboard || true
