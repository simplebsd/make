install:
	@echo
	@echo [Install Minio package]
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg install -y minio
	@echo

	@echo [Configure Minio, enable and start service]
	@echo
	mkdir -p /usr/local/minio
	chown -R minio:minio /usr/local/minio
	sysrc minio_enable="YES"
	sysrc minio_disks="/usr/local/minio"
	service minio start

deinstall:
	@echo
	@echo [Stop and disable Minio service]
	service minio stop || true
	sysrc -x minio_enable
	@echo

	@echo [Uninstall Minio package]
	pkg del -y minio || true
	pw userdel minio -r || true
