install:
	mkdir -p /usr/local/minio
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg install -y minio
	chown -R minio:minio /usr/local/minio
	sysrc minio_enable="YES"
	sysrc minio_disks="/usr/local/minio"
	service minio start

deinstall:
	service minio stop || true
	sysrc -x minio_enable
	pkg del -y minio || true
	pw userdel minio -r || true
