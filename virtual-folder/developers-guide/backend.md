# Backend

Backend components consist of metadataservice \(C\# .NET\), Bash scripts and configuration files.

`/var/lib/westlife/metadata.sqlite` SQLite DB is used to store metadata about user, datasets and tasks.
 
`/etc/westlife/` contains configuration of VF. Note that some user's specific entities are encrypted thus configuration should be moved together with database file in order to preserve user's metadata of connected data storage.

`/opt/virtualfolder/MetadataService` contains binaries build from source code. Otherwise the binaries are taken from `/cvmfs/west-life.egi.eu/software/virtualfolder`.

## Metadata service
Provides REST api for frontend and client.
It is `systemd` daemon configured in `/etc/systemd/system/westlife-metadata.service`
source codes at `conf-template/etc/systemd/system/westlife-metadata.service`.

It's possible to ammend these environment variables
```bash
/etc/systemd/system/westlife-metadata.service
...
[Service]
Environment=VF_VRE_API_URL=http://localhost/api/ # location of VRE api, if present 
Environment=VF_STORAGE_DIR=/srv/virtualfolder/   # filesystem location where user's virtual folders are mounted 
Environment=VF_SCRIPTS_DIR=/opt/virtualfolder/scripts # location of scripts where virtualfolder is installed
Environment=VF_ALLOW_FILESYSTEM=true             # true - allows filesystem provider, false= 
Environment=VF_ALLOW_MODULES=true                # true - enables testing modules
Environment=VF_ALLOW_LAB=true                    # true - enables Jupyter LAB task
Environment=VF_ALLOW_NOTEBOOK=true               # true -enables Jupyter notebook tastk
Environment=VF_DATABASE_FILE=/var/lib/westlife/metadata.sqlite # database file location
EnvironmentFile=/etc/westlife/metadata.key       # configuration file 
```
Log files are by default at `/var/log/westlife` directory.

## Further doc

[http://internal-wiki.west-life.eu/index.php?title=D6.1](http://internal-wiki.west-life.eu/index.php?title=D6.1)

