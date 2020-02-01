install:
	# Install required packages
	pkg ins -y wget openjdk8 tomcat-native
	
	# Create Jira user
	pw useradd jira -s /bin/sh -d /usr/local/jira || true

	# Create Jira home and Jira install directories
	mkdir -p /usr/local/jira
	mkdir -p /usr/local/share/jira

	# Download Jira tar.gz archive
	wget -P /tmp/ https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-software-8.6.1.tar.gz

	# Extract tar.gz archive to Jira install directory
	tar xzvf /tmp/atlassian-jira-software-8.6.1.tar.gz --strip 1 -C /usr/local/share/jira

	# Add permissions for Jira install directory to JIra user
	chown -R jira:jira /usr/local/share/jira

	# Add rc.d Jira file
	cp templates/jira /usr/local/etc/rc.d/

	# Add permissions to rc.d Jira file
	chmod u-w,ugo+x /usr/local/etc/rc.d/jira

	# Enable and start Jira
	sysrc jira_enable="YES"
	service jira start

deinstall:
	# Stop and disable Jira
	service jira stop || true
	sed -i '' '/jira/d' /etc/rc.conf

	# Delete rc.d Jira file
	rm -f /usr/local/etc/rc.d/jira

	# Delete Jira home and install directories
	rm -rf /usr/local/jira
	rm -rf /usr/local/share/jira

	# Delete Jira user
	pw userdel jira -r || true

	# Delete additional packages
	pkg del -y wget openjdk8 tomcat-native
