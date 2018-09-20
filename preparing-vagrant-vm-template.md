# Preparing vagrant VM template

### Download ISO 

As a base for most VM templates, Scientific Linux is used. Download the latest version from [http://ftp1.scientificlinux.org/linux/scientific/7x/x86\_64/iso/](http://ftp1.scientificlinux.org/linux/scientific/7x/x86_64/iso/) 

recommended is  Network installation ISO - SL-\*-netinst.iso

### Install minimal system

In installation packages - select Minimal system.

### Define root and vagrant user

Set root password \(vagrant\) and create new user \(vagrant:vagrant\)

### Post-installation script for non-GUI environment

Log-in as root, and download and execute script at [https://gist.githubusercontent.com/TomasKulhanek/21e4544823dfcd181b3d0787a5b525a1/raw/1e8ff9ff25c532cb2945607dbe7d4c0082e5ffe7/sl7prepareminimalvagrant.sh](https://gist.githubusercontent.com/TomasKulhanek/21e4544823dfcd181b3d0787a5b525a1/raw/1e8ff9ff25c532cb2945607dbe7d4c0082e5ffe7/sl7prepareminimalvagrant.sh) - which is shortcuted to [https://bit.ly/2NV019y](htps://bit.ly/2NV019y).

```
$ bash <(curl -L https://bit.ly/2NV019y)
```

### Post-installation script for GUI-environment \(X-Window system\)

Log-in as root and download and execute following script:

```
$ bash <(curl -L http://bit.ly/2QMD1ba)
```



