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
# location of VRE api, if present
Environment=VF_VRE_API_URL=http://localhost/api/ 
# filesystem location where user's virtual folders are mounted 
Environment=VF_STORAGE_DIR=/srv/virtualfolder/    
# location of scripts where virtualfolder is installed, 
# mountb2drop and other scripts should be present
Environment=VF_SCRIPTS_DIR=/opt/virtualfolder/scripts
# true - allows filesystem provider (access to VM, 
# recommended for single deployment), false - default  
Environment=VF_ALLOW_FILESYSTEM=true
# true - enables testing modules              
Environment=VF_ALLOW_MODULES=true                
# true - enables Jupyter LAB task, it allows to execute user's script on VM
# recommended for private deployment for trusted users
Environment=VF_ALLOW_LAB=true                    
# true -enables Jupyter notebook tast, it allows to execute user's script on VM
# recommended for private deployment for trusted users
Environment=VF_ALLOW_NOTEBOOK=true               
# location of database file for metadata
Environment=VF_DATABASE_FILE=/var/lib/westlife/metadata.sqlite
# configuration file with other environment variables, keys, etc. 
EnvironmentFile=/etc/westlife/metadata.key        
```
Log files are by default at `/var/log/westlife` directory.

## Further doc

[http://internal-wiki.west-life.eu/index.php?title=D6.1](http://internal-wiki.west-life.eu/index.php?title=D6.1)

