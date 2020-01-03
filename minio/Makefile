install:
	pkg ins -y minio
	sysrc minio_enable="YES"
	service minio start

deinstall:
	service stop minio || true
	sed -i '' '/minio_enable/d' /etc/rc.conf
	pkg del -y minio
	pw userdel minio -r || true
	rm -rf /usr/local/etc/minio
