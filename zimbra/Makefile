install:
	@echo
	@echo [Update repo and install Git, Screen, Bash]
	@echo
	env ASSUME_ALWAYS_YES=YES pkg update
	pkg ins -y nginx fcgiwrap git cgit
	@echo

	@echo [Add Zimbra user]
	@echo
	pw useradd -c "Zimbra user" -s /usr/local/bin/bash
	@echo

deinstall:
