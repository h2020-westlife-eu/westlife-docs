# Cloud installation

Images to deploy Virtual Folder are maintained and distributed via appdb.egi.eu.

1. [https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm](https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm) - The standard OVA \(open virtual appliance\) image can be used to deploy West-Life VM into e.g. OpenNebula cloud environment.
2. [https://appdb.egi.eu/store/vappliance/west.life.vm](https://appdb.egi.eu/store/vappliance/west.life.vm) - RAW image can be used to deploy West-Life VM into OpenStack cloud environment

Both images are small \(18 MB, 23 MB respectively\) containing only CernVM 4 bootloader, which boots into standard Scientific Linux \(currently version 7.3\) and contextualizes it with West-Life specific software:

* Virtual Folder – private instance
* VRE – \(optional\) private instance delivering multiuser environment
* CCP4 – \(optional available after license agreement\) software suite containing software tools to process data produced by X-ray crystallography methods
* Scipion – image processing framework to process data produced mainly by electron microscopy methods.
* WeNMR – software tools to process data mainly of NMR methods

## Downloading VM image

You may download the latest West-life VM in the OVA compatible format from

[https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm](https://appdb.egi.eu/store/vappliance/d6.1.virtualfoldervm)

![](../../.gitbook/assets/downloadappdb.gif)

Alternatively you may use the RAW image for deployment into OpenStack at [https://appdb.egi.eu/store/vappliance/west.life.vm](https://appdb.egi.eu/store/vappliance/west.life.vm)

Use the downloaded image to deploy it to prefered provider. You may deploy the image in EGI resources - follow EGI documentation.

## Contextualization

OVA image for OpenNebula contains by default contextualization, RAW image for OpenStack contains reference to cloud-init file which needs to be provided in order to boot Virtual Folder and related software during first boot. By default the image contextualization is set to use direct connection to CernVM-FS repositories. If you have local squid proxy server, it is very recommended to configure it in contextualization script. 

```text
[ucernvm-begin]
cvmfs_http_proxy=http://<host>:<port>
[ucernvm-end]

[cernvm]
proxy = http://<host>:<port>;DIRECT

```

Further information about contextualization of CernVM can be found at [https://cernvm.cern.ch/portal/contextualisation](https://cernvm.cern.ch/portal/contextualisation)

