# Integration with SSO

In order to integrate with West-Life SSO, you need your `sp-metadata,idp-metadata,sp_key`and`sp_cert` files.

* If you have it from previous installation, then put it next to the VagrantFile, or put them into virtual machine/container folder `/vagrant` and exectue this script in bash within VF machine or container:
```bash
export SSO_DEPLOYMENT=1
wp6-virtualfolder/bootstrap/bootstrapsso.sh
```

* If you don't have keys and certificates yet, then execute this script in bash within VF machine or container:
```bash
export SP_IDENTIFICATION=http://[your public domain of repository]
export SP_ENDPOINT=http://[your public domain of repository]/mellon
export SSO_DEPLOYMENT=1
wp6-virtualfolder/bootstrap/bootstrapsso.sh
```

* Find the generated file `sp-metadata.xml` and register it with West-Life SSO administrator - send it to westlife-aai@ics.muni.cz 

{% hint style="info" %}
Note that 

`export SP_IDENTIFICATION=http://[your public domain of repository]` defines id of service provider - your instance of virtual folder, so to be unique put your public domain, e.g. `https://portal.west-life.eu/`

`export SP_ENDPOINT=http://[your public domain of repository]/mellon` defines endpoint where mellon plugin is available in VF installation, e.g. `https://portal.west-life.eu/mellon/`

`export SSO_DEPLOYMENT=1` just sets trigger for further script 

`bootstrapsso.sh` configures VF with SSO, installs mellon plugin into apache, configures aliases, if the keys are not present in `/vagrant/sp_key.pem` then the keys are generated based on SP_IDENTIFICATION and SP_ENDPOINT, and puts generated keys into expected folders into /etc/...
{% endhint %}