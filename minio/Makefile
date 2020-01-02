install:
	pkg ins -y minio
	echo 'minio_enable="YES"' >> /etc/rc.conf
	service minio start

deinstall:
	service stop minio || true
	sed -i '' '/minio_enable/d' /etc/rc.conf
	pkg del -y minio
	pw userdel minio -r || true
	rm -rf /usr/local/etc/minio
	rm -rf /var/db/minio
