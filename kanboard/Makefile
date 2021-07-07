install:
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y nginx php80 php80-kanboard
	cp templates/nginx.conf /usr/local/etc/nginx/
	chown -R www:www /usr/local/www/kanboard
	sysrc nginx_enable="YES"
	sysrc php_fpm_enable="YES"
	service nginx start
	service php-fpm start

deinstall:
	service nginx stop || true
	service php-fpm stop
	sysrc -x nginx_enable
	sysrc -x php_fpm_enable
	pkg del -y nginx php80 php80-kanboard || true
