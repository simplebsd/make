install:
	@echo
	@echo [Add pf.conf file]
	@echo
	cp pf.conf /etc/
	@echo

	@echo [Enable and start PF]
	@echo
	sysrc pf_enable="YES"
	sysrc pf_rules="/etc/pf.conf"
	service pf start
