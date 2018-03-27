# Virtual Machines

The repository https://github.com/h2020-westlife-eu/wp6-vm.git
 contains various configuration for development and testing purposes. Prerequisites:

 1. Vagrant - tool for automation of virtual machine deployment. 
  1. For MS Windows - Download and install vagrant from https://www.vagrantup.com/  (tested on/recommended version vagrant 1.9.6, vagrant 2.0.3 on Windows requires updated Powershell, e.g. via Windows Management Framework WMF 5.1 https://www.microsoft.com/en-us/download/details.aspx?id=54616) 
  2. For Linux - use prefered package management
     1. Ubuntu:```apt install vagrant```
     2. Centos(RHEL):```yum install vagrant```
 2. Virtualbox - VM stack. 
   1. For MS Windows - Download and install virtualbox https://www.virtualbox.org/wiki/Downloads
 (tested on/recommended version Virtualbox 5.1.22, for vagrant 2.0.3 tested on VirtualBox 5.2.8)
   2. For Linux - use preferred package management. 
      1. Ubuntu: ```apt install virtualbox```
      2. Centos(RHEL): ```yum install virtualbox```

## Brief instruction using Vagrant

The Vagrant tool configures and bootstraps virtual machine in Virtualbox.
Brief instructions are:

```
git clone https://github.com/h2020-westlife-eu/wp6-vm.git

cd wp6-vm/[selected_config]

vagrant up
```

![](/virtualfolder/assets/VMVagrantUp.gif)

This clones special repository with various configuration. After succesfull `vagrant up`, there should be message `BOOSTRAP FINISHED, VM prepared to use` or similar, read the docs specific to the software/service.

If the application in virtual machine provides web application, you can access it with web browser using port 8080\(check VagrantFile or vagrant log for exact port number being forwarded\)

```
http://localhost:8080/
```
You can access the desktop of the VM by going into VirtualBox or you can log into the VM as user vagrant using
```
vagrant ssh
```
After you finish your work, you can destroy the VM and release resources by: 
```
vagrant destroy
```

## Detailed instruction


Download or clone metarepository [ZIP (4kB)](https://github.com/h2020-westlife-eu/wp6-vm/archive/master.zip) unzip it into some [wp6-vm directory] or clone the main repository https://github.com/h2020-westlife-eu/wp6-vm.git.

**1.** Open command-line (e.g. cmd, cygwin or terminal) and cd to directory where wp6-vm is unzipped/cloned
     
    cd [wp6-vm directory]/[selected configuration]

These configurations are available:
### Standalone from Source Codes (default)
This is based on CernVM 4.0 micro image which boots into Scientific Linux 7. Initial VM image size = 18MB, during boot and bootstrap downloads 658 MB. This is preferred option as CernVM distrtomaibutes most updated SL7 with recent security updates, so either restart or ```cernvm-update -a``` is required occasionally.
```bash
cd wp6-vm/vf-standalone-src/
vagrant up
```

### Standalone from Binaries (distributed via cvmfs).
The same as above - but Virtual Folder is not compiled from sources -boots from `\cvmfs\`. This option is faster, the last stable release is distributed.
```bash
cd wp6-vm/vf-standalone-bin/
vagrant up
```

### Standalone from Source Codes on Scientific Linux 7 (~RHEL 7) 
It is based on minimal Scientific Linux 7 - no dependency on online repositories at all. Initial VM image size = 665 MB, boot and bootstrap download 320 MB. Recommended for preparing off-line deployment.
```
cd wp6-vm/vf-standalone-src-sl7/
vagrant up
```

### Test configurations 
These are currently in testing stage, not guaranted to be working.
```
cd wp6-vm/test-...
vagrant up
```
### Base box update
Optionally, if you have used west-life VM before, you may remove previous VM and update the vagrant box cache

    vagrant destroy
    vagrant box update    
        

### Deploy development branch
By default, the master branch from sources are cloned into VM and VM is booted. To change it, edit the bootstrapsources.sh file and uncomment/edit the following three lines (change 'dev' with a desired git branch):

    # optional switch to branch
    cd west-life-wp6
    git checkout dev
    cd ..
### Virtual Folder enable multiuser environment
By default, Virtualfolder in VM will contain single user environment - no login is required. To enable multiuser environment with VRE, edit bootstrapsources.sh file and uncomment the following line. Default user for VF will then be vagrant/vagrant:

    export PORTAL_DEPLOYMENT=1  

### Base Box 
The following base boxes are used: 
- Scientific Linux 7 with minimal GUI (~600MB), some additional packages are downloaded during bootstrap. Bootstrap takes about 4 mins.
- CernVM 4  (~18MB), after boot it will download additional 200-300 MB.

## Usage

After 'vagrant up' finished, the new virtual machine can be accessed via web browser (port 8080 is by default forwarded to VM, check VagrantFile or vagrant log for exact port number)

    http://localhost:8080/

Default login name to VRE is vagrant/vagrant.
    
Files of the current working directory of host are mounted into the guest <code>/vagrant</code>.

You can access the guest by SSH (default port 2222 is forwarded to VM)

    vagrant ssh

or access GUI in virtualbox (username/password: vagrant/vagrant).

## Uninstallation - Cleaning
*6.* After testing you may, stop (halt) the VM:
   
    vagrant halt
    
*7.* If you will not use the VM anymore, you can delete (destroy) the VM:
    
    vagrant destroy

    
## Release Notes

  * 27/06/2017 - added variation of Vagrant scripts for different deployment type
  * 03/05/2017 - works with Vagrant version 1.9.3 and bellow or 1.9.5 and above. Vagrant version 1.9.3 needs to use different Vagrantfile, Vagrant version 1.9.4 has bug preventing to bootstrap the VM.
  * 25/11/2016 - Updated vagrant boxes to use uCernVM 2.7.7 bootloader, updated OVA images in https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm/vaversion/latest and vagrant boxes, do "vagrant box update", bug fixes, consolidated initial web page and design, fixed/added background services
  * 26/10/2016 - moved VagrantFile to new repository https://github.com/h2020-westlife-eu/wp6-vm, updated base box with uCernVM2.7.4 bootloader for CernVM 4 fixes security bug 'dirty COW' and aufs bug in kernel, https://atlas.hashicorp.com/westlife-eu, 
   * tested on Windows 7 64 bit, vagrant 1.8.6 + VirtualBox 5.1.6, vagrant 1.8.1, 1.8.4, VirtualBox 5.0.26, note vagrant < 1.8.6 requires VirtualBox 5.0.x, doesn't require VirtualBox extension pack, download from https://www.virtualbox.org/wiki/Download_Old_Builds_5_0 
   * tested on Ubuntu 14.04 LTS, (default vagrant 1.4.3 needs to be updated to 1.8.6), default VirtualBox 4.3.36 works
