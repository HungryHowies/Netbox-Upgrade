# Netbox Upgrade
NetBox Upgrade
The Netbox upgrade script does the following action, ensure all files are copied in to a safe
location. These instruction will install the latest stable version of Netbox.
Destroys and rebuilds the Python virtual environment <--- This was my issue when placing
the XML file in /opt/netbox directory.
Installs all required Python packages (listed in requirements.txt)
Installs any additional packages from local_requirements.txt
Applies any database migrations that were included in the release
Builds the documentation locally (for offline use)
Collects all static files to be served by the HTTP service
Deletes stale content types from the database
Deletes all expired user sessions from the database
I followed these exact commands without a issue.
Download and extract the latest version:
## Set $NEWVER to the NetBox version being installed
Copy local_requirements.txt, configuration.py, (or any other files that are being used) from the
current installation to the new version:
# Set $OLDVER to the NetBox version currently installed
NEWVER=3.5.0
wget https://github.com/netbox-community/netbox/archive/v$NEWVER.tar.gz
sudo tar -xzf v$NEWVER.tar.gz -C /opt
sudo ln -sfn /opt/netbox-$NEWVER/ /opt/netbox
OLDVER=3.4.9
sudo cp /opt/netbox-$OLDVER/local_requirements.txt /opt/netbox/
sudo cp /opt/netbox-$OLDVER/netbox/netbox/configuration.py /opt/netbox/netbox/netbox/
sudo cp /opt/netbox-$OLDVER/netbox/netbox/ldap_config.py /opt/netbox/netbox/netbox/
