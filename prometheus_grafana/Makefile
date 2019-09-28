install:
	pkg ins -y prometheus node_exporter grafana6
	echo 'prometheus_enable="YES"' >> /etc/rc.conf
	echo 'node_exporter_enable="YES"' >> /etc/rc.conf
	echo 'grafana_enable="YES"' >> /etc/rc.conf
	echo "  - job_name: 'node_exporter'" >> /usr/local/etc/prometheus.yml
	echo '    scrape_interval: 5s' >> /usr/local/etc/prometheus.yml
	echo '    static_configs:' >> /usr/local/etc/prometheus.yml
	echo "    - targets: ['localhost:9100']" >> /usr/local/etc/prometheus.yml
	service prometheus start
	service node_exporter start
	service grafana start

deinstall:
	service grafana stop || true
	service node_exporter stop || true
	service prometheus stop || true
	sed -i '' '/grafana_enable/d' /etc/rc.conf
	sed -i '' '/node_exporter_enable/d' /etc/rc.conf
	sed -i '' '/prometheus_enable/d' /etc/rc.conf
	pkg del -y prometheus node_exporter grafana6 || true
	pw userdel grafana -r || true
	pw userdel prometheus -r || true
	rm -f /usr/local/etc/prometheus.yml || true
	rm -rf /var/tmp/prometheus/ || true
	rm -rf /var/db/grafana || true
