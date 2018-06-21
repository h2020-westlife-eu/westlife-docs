# Backend

## Backend

Backend components consist of metadataservice \(C\# .NET\), Bash scripts and configuration files.

`/var/lib/westlife/metadata.sqlite` SQLite DB is used to store metadata about user, datasets and tasks.
 
`/etc/westlife/` contains configuration of VF. Note that some user's specific entities are encrypted thus configuration should be moved together with database file in order to preserve user's metadata of connected data storage.

`/opt/virtualfolder/MetadataService` contains binaries build from source code. Otherwise the binaries are taken from `/cvmfs/west-life.egi.eu/software/virtualfolder`.



## Further doc

[http://internal-wiki.west-life.eu/index.php?title=D6.1](http://internal-wiki.west-life.eu/index.php?title=D6.1)

