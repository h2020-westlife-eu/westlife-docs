# Preparing vagrant VM template

### Download ISO 

As a base for most VM templates, Scientific Linux is used. Download the latest version from [http://ftp1.scientificlinux.org/linux/scientific/7x/x86\_64/iso/](http://ftp1.scientificlinux.org/linux/scientific/7x/x86_64/iso/) 

recommended is  Network installation ISO - SL-\*-netinst.iso

### Install minimal system

In installation packages - select Minimal system.

### Define root and vagrant user

Set root password \(vagrant\) and create new user \(vagrant:vagrant\)

### Post-installation script

Log-in as root, and download and execute one of the following script:

1. for non-GUI environment: `bash <(curl -L https://bit.ly/2NV019y)`
2. for GUI environment: `bash <(curl -L https://bit.ly/2QKQUH0)`

