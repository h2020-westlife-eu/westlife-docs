## Backend

Backend components consist of metadataservice \(C\# .NET\), common portal \(Python, Django\) and Bash scripts and configuration files.

## Database configuration

`/home/vagrant/.westlife` contains database configuration and key to access secured content. In order to backup or move configuration, it needs to be moved together. 

## Release Notes

* 25/11/2016 - Updated vagrant boxes to use uCernVM 2.7.7 bootloader, updated OVA images in [https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm/vaversion/latest](https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm/vaversion/latest) and vagrant boxes, do "vagrant box update", bug fixes, consolidated initial web page and design, fixed/added background services
* 26/10/2016 - moved VagrantFile to new repository [https://github.com/h2020-westlife-eu/wp6-vm](https://github.com/h2020-westlife-eu/wp6-vm), updated base box with uCernVM2.7.4 bootloader for CernVM 4 fixes security bug 'dirty COW' and aufs bug in kernel, [https://atlas.hashicorp.com/westlife-eu](https://atlas.hashicorp.com/westlife-eu), 
  * tested on Windows 7 64 bit, vagrant 1.8.6 + VirtualBox 5.1.6, vagrant 1.8.1, 1.8.4, VirtualBox 5.0.26, note vagrant &lt; 1.8.6 requires VirtualBox 5.0.x, doesn't require VirtualBox extension pack, download from [https://www.virtualbox.org/wiki/Download\_Old\_Builds\_5\_0](https://www.virtualbox.org/wiki/Download_Old_Builds_5_0) 
  * tested on Ubuntu 14.04 LTS, \(default vagrant 1.4.3 needs to be updated to 1.8.6\), default VirtualBox 4.3.36 works

## Further doc

[http://internal-wiki.west-life.eu/w/index.php?title=D6.1](http://internal-wiki.west-life.eu/w/index.php?title=D6.1)

