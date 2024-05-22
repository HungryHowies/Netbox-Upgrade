##Netbox v4.0.2 Upgrade Procedure.

Netbox v4.0.x has a few release changes. New packages need to be upgrade.

### Prerequisite:

* Ubunt-22.0.4
* All updates/upgrade completed
* NetBox-v3.7.8 installed
* Python-v3.10.x installed

### Upgrade Netbox SAML Plugin

Update Instance

```
apt update && apt upgrade
```

Execute the following command to upgrade netbox-plugin-auth-saml2 plugin.

```
pip3 install --upgrade --user netbox-plugin-auth-saml2
```
Check  new version is installed

```
pip3 list
```
![image](https://github.com/HungryHowies/Netbox-Upgrade/assets/22652276/445a65e2-47ac-43ea-ba5d-e02f946325a1)



### The Netbox upgrade script does the following action,  

* Destroys and rebuilds the Python virtual environment
* Installs all required Python packages (listed in requirements.txt)
* Installs any additional packages from local_requirements.txt
* Applies any database migrations that were included in the release
* Builds the documentation locally (for offline use)
* Collects all static files to be served by the HTTP service
* Deletes stale content types from the database
* Deletes all expired user sessions from the database

### Download and install NetBox-v4.0.2 package and Dependencies. 

NetBox releases are located [here](https://github.com/netbox-community/netbox/releases).

Download new release package.

```
wget https://github.com/netbox-community/netbox/archive/refs/tags/v4.0.2.tar.gz
```

Extract new version to /opt directory.

```
tar -xzf v4.0.2.tar.gz -C /opt
```

Copy the following files over.

```
sudo cp /opt/netbox-3.7.8/local_requirements.txt /opt/netbox-4.0.2/
```
```
sudo cp /opt/netbox-3.7.8/netbox/netbox/configuration.py /opt/netbox-4.0.2/netbox/netbox/
```

```
sudo cp /opt/netbox-3.7.8/DCIM.xml /opt/netbox-4.0.2/DCIM.xml
```

Replicate your uploaded media.

```
sudo cp -pr /opt/netbox-3.7.8/netbox/media/ /opt/netbox-4.0.2/netbox/
```

copy or link any custom scripts and reports that you've made.

```
sudo cp -r /opt/netbox-3.7.8/netbox/scripts /opt/netbox-4.0.2/netbox/
```
```
sudo cp -r /opt/netbox-3.7.8/netbox/reports /opt/netbox-4.0.2/netbox/
```

Copy gunicorn.py file.

```
sudo cp /opt/netbox-3.7.8/gunicorn.py /opt/netbox-4.0.2/
```

## Install Dependencies

```
pip install -r /opt/netbox-4.0.2/requirements.txt
```

Link netbox-3.7.3 directory to /opt/netbox directory.

```
sudo ln -sfn /opt/netbox-4.0.2/ /opt/netbox
```

###  Upgrade Script.

Change directory.

```
cd /opt/netbox
```

Execute upgrade script.

```
sudo ./upgrade.sh
```

Once completed without issues reload Systemd.

```
systemctl daemon-reload
```

Restart services.

```
sudo systemctl restart netbox netbox-rq
```

Check service status.

```
sudo systemctl status  netbox netbox-rq
```
 
