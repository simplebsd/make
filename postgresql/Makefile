install:
	# Install required PostgreSQL
	pkg ins -y postgresql12

	# Enable and init PostgreSQL DB
	sysrc postgresql_enable="YES"
	service postgresql initdb

	# Add PostgreSQL conf files
	cp templates/postgresql.conf /var/db/postgres/data12/
	cp templates/pg_hba.conf /var/db/postgres/data12/

	# Create log file
	touch /var/db/postgres/data12/postgresql.log

	# Start Postgresql DB
	service postgresql start

deinstall:
	# Stop and disable PostgreSQL database
	service postgresql stop

	# Uninstall PostgreSQL database
	pkg del -y postgresql12
