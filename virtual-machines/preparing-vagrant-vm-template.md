# Preparing vagrant VM template

The following section describes how to prepare VM template from scratch, e.g. to be used as base vagrant box or as a base VM template. It's not needed to perform these steps when installing any products, however, might be usefull to maintain version of base OS.

## Scientific Linux 7

### Download ISO 

As a base for most VM templates, Scientific Linux is used. Download the latest version from [http://ftp1.scientificlinux.org/linux/scientific/7x/x86\_64/iso/](http://ftp1.scientificlinux.org/linux/scientific/7x/x86_64/iso/) 

recommended is  Network installation ISO - SL-\*-netinst.iso

### Install minimal system

In installation packages - select Minimal system.

### Define root and vagrant user

Set root password \(vagrant\) and create new user \(vagrant:vagrant\)

### Post-installation script

Log-in as root, and execute one of the following script:

1. for non-GUI environment: `bash <(curl -L https://bit.ly/2xDpLwR)`
2. for GUI environment: `bash <(curl -L http://bit.ly/2GfrE7z)`

### Create box

launch vagrant script to package box. Expecting the virtual machine name is `my-sl7-virtualmachine`

```text
vagrant package --output sl7mini.box --base my-sl7-virtualmachine
```

Explanation:

* `package` instruct vagrant to get virtual machine from virtual box and package it into separate file
* `--output sl7mini.box` writes the result to file named as sl7mini.box
* `--base my-sl7-install` takes VirtualBox virtual machine named my-sl7-virtualmachine

## CernVM 4

Download CernVM4 image [https://cernvm.cern.ch/portal/downloads](https://cernvm.cern.ch/portal/downloads) for vagrant.

