# Netbox Upgrade

These instruction will install Netbox-v3.7.3 from Netbox-v3.6.5 and explains how to download the new Netbox package and use the "Upgrade" script.

The Netbox upgrade script does the following action, ensure all files are copied in to a safe location. 

* Destroys and rebuilds the Python virtual environment
* Installs all required Python packages (listed in requirements.txt)
* Installs any additional packages from local_requirements.txt
* Applies any database migrations that were included in the release
* Builds the documentation locally (for offline use)
* Collects all static files to be served by the HTTP service
* Deletes stale content types from the database
* Deletes all expired user sessions from the database

NetBox releases are located [here](https://github.com/netbox-community/netbox/releases).

Change directory

```
cd /home
```

Download new release package.

```
wget https://github.com/netbox-community/netbox/archive/refs/tags/v3.7.3.tar.gz
```

Extract new verion to /opt directory.

```
tar -xzf v3.7.3.tar.gz -C /opt
```

Link netbox-3.7.3 directory to /opt/netbox directory.

```
sudo ln -sfn /opt/netbox-3.7.3/ /opt/netbox
```

Copy the following files over to the new release.

```
sudo cp /opt/netbox-3.6.5/local_requirements.txt /opt/netbox/
```
```
sudo cp /opt/netbox-3.6.5/netbox/netbox/configuration.py /opt/netbox/netbox/netbox/
```

```
sudo cp /opt/netbox-3.6.5/DCIM.xml /opt/netbox/DCIM.xml
```


Copy local_requirements.txt and  configuration.py from the current installation to the new version.

```
sudo cp /opt/netbox-3.6.5/local_requirements.txt /opt/netbox/
```
```
sudo cp /opt/netbox-3.6.5/netbox/netbox/configuration.py /opt/netbox/netbox/netbox/
```

Replicate your uploaded media.

 ```
sudo cp -pr /opt/netbox-3.6.5/netbox/media/ /opt/netbox/netbox/
```

copy or link any custom scripts and reports that you've made.

```
sudo cp -r /opt/netbox-3.6.5/netbox/scripts /opt/netbox/netbox/
```
```
sudo cp -r /opt/netbox-3.6.5/netbox/reports /opt/netbox/netbox/
```

Copy gunicorn.py file.

```
sudo cp /opt/netbox-3.6.5/gunicorn.py /opt/netbox/
```

Run the Upgrade Script.

Change directory

```
cd /opt/netbox
```

Execute Upgrade script

```
sudo ./upgrade.sh
```

Once completed without issues reload Systemd

```
systemctl daemon-reload
```

Retsart services

```
sudo systemctl restart netbox netbox-rq
```

Check status

```
sudo systemctl status  netbox netbox-rq
```

Bottom right corner the new version should be shown.
