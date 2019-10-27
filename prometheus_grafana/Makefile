install:
	pkg ins -y prometheus node_exporter grafana6 alertmanager
	sysrc prometheus_enable="YES"
	sysrc node_exporter_enable="YES"
	sysrc grafana_enable="YES"
	sysrc alertmanager_enable="YES"
	
	cp prometheus.yml /usr/local/etc/prometheus.yml
	cp alertmanager.yml /usr/local/etc/alertmanager/alertmanager.yml

	service prometheus start
	service node_exporter start
	service grafana start
	service alertmanager start

deinstall:
	service grafana stop || true
	service node_exporter stop || true
	service prometheus stop || true
	service alertmanager stop || true
	sed -i '' '/grafana_enable/d' /etc/rc.conf
	sed -i '' '/node_exporter_enable/d' /etc/rc.conf
	sed -i '' '/prometheus_enable/d' /etc/rc.conf
	sed -i '' '/alertmanager_enable/d' /etc/rc.conf
	pkg del -y prometheus node_exporter grafana6 alertmanager || true
	pw userdel grafana -r || true
	pw userdel prometheus -r || true
	rm -f /usr/local/etc/prometheus.yml || true
	rm -rf /var/tmp/prometheus/ || true
	rm -rf /var/db/grafana || true
	rm -rf /usr/local/etc/alertmanager || true
