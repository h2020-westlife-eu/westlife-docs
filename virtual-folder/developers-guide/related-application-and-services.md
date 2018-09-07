# Related application and services

Virtual folder can be installed with optional software, application and services.

### Jupyter binary installation

You may use binary installation from `/cvmfs/west-life.egi.eu` repository, to do that, create link `/opt/jupyter` to e.g. the latest version:

```bash
ln -s /cvmfs/west-life.egi.eu/software/jupyter/latest /opt/jupyter
```

Then enable the jupyter tasks in metadata service configuration `/etc/systemd/system/westlife-metadata.service` by adding `VF_ALLOW_LAB=true` `VF_ALLOW_NOTEBOOK=true.` See Manual configuration for details.

### Jupyter local installation 

In order to install Jupyter in local installation during bootstrap of VF - set local variable `ALLOW_JUPYTER=1`before launching bootstrap, i.e.:

```bash
# define where VF sources are located 
# export WP6SRC=/vagrant
export ALLOW_JUPYTER=1 # enable jupyter
$WP6SRC/bootstrap/bootstrap.sh
```

### Jupyter manual installation

Jupyter notebook and Jupyter Lab can be installed manually, see/edit the `bootstrap/bootstrapJupyter.sh` and set local variable `ALLOW_JUPYTER=1`before launching this script: 

```bash
# define where VF sources are located 
# export WP6SRC=/vagrant
export ALLOW_JUPYTER=1 # enable jupyter
$WP6SRC/bootstrap/bootstrapJupyter.sh
```

### Integration to Metadata service

In order to enable Jupyter from binary installation or after manual installation:

* check that python environment is available in path `/opt/jupyter` e.g. as symbolic link to proper location where it was.
* check/enable jupyter notebook and jupyter lab task in metadata service configuration `/etc/systemd/system/westlife-metadata.service` by adding `VF_ALLOW_LAB=true` `VF_ALLOW_NOTEBOOK=true`:

  ```text
  [Service]
  ...
  Environment=VF_ALLOW_LAB=true
  Environment=VF_ALLOW_NOTEBOOK=true
  ...
  ```



