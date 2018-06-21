# Developer's guide

Get source codes directly from [github.com/h2020-westlife-eu/virtualfolder](https://github.com/h2020-westlife-eu/virtualfolder), or use option to install virtual machine from source codes described at [Virtual Machines](../../virtual_machines.md). 


## Source code structure

```text
/vmcontext          # helper configuration and scripts for virtual machines creation         
/wp6-virtualfolder  # source code for virtual folder
LICENSE             # license
README.md           # brieaf installation instructions
RELEASE.md          # instruction to make release
Vagrantfile         # vagrant configuration to prepare VM
bootstrap.sh        # bootstrap script, will install VF in clean VM
```

The source code of VF components in `/wp6-virtualfolder` are
```
/bootstrap          # various bootstrap scripts for components
/conf-template      # default templates for linux configuration files
/scipion            # minor ammendments for scipion installation inside local VF VM
/scripts            # bash scripts used by components of VF
/singlevre/api      # static proxy of vre component in single user environment
/src/WP6Service2    # backend in C# (.NET and ServiceStack) of metadataservice
/www                # frontend in HTML and Javascript (ECMAScript 6 and Aurelia)
```



## Remove unwanted commits from git tree

Use it when accidental commit was done and to remove all sensitive files.

[https://help.github.com/articles/removing-sensitive-data-from-a-repository/](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)

