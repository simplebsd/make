install:
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y nginx fcgiwrap git cgit
	pw groupadd -n git
	pw useradd -n git -c "Git user" -s /usr/local/libexec/git-core/git-shell -d /git
	cp templates/nginx.conf /usr/local/etc/nginx/
	sysrc nginx_enable="YES"
	sysrc fcgiwrap_enable="YES"
	sysrc fcgiwrap_user="www"
	sysrc fcgiwrap_socket_owner="www"
	sysrc fcgiwrap_socket_group="www"
	service nginx start
	service fcgiwrap start
