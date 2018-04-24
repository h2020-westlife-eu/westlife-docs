# Installation guide

## From source codes

[Vagrant tool](https://www.vagrantup.com/downloads.html) and [Virtualbox](https://www.virtualbox.org/wiki/Downloads) is required for installing Repository from source codes. Tested on Vagrant \(1.9.6 and 2.0.3\) and VirtualBox \(5.1.30 and 5.2.8\).

External registration is needed with West-Life SSO and ARIA for integrating single sign on and project proposal import. Demo registration package can be provided upon request.

{% hint style="warning" %}
In order to integrate with West-Life SSO, you need your `sp-metadata,idp-metadata,sp_key `and` sp_cert` files. 

If you have it from previous installation, then put it next to the VagrantFile - these will be reused. 

If you don't have, these will be generated and you need to register them with West-Life SSO. Edit the bootstrap.sh file and change values of variables to your hostname and registered identification with West-Life SSO.

`SP_IDENTIFICATION=http://[your public domain for repository] SP_ENDPOINT=http://[your public domain for repository]/mellon` 

Register the generated files with West-Life SSO - send them to westlife-aai@ics.muni.cz
{% endhint %}

{% hint style="warning" %}
In order to have working ARIA integration, you need your fully qualified domain name registered with Instruct via [https://www.structuralbiology.eu/contact-us/](https://www.structuralbiology.eu/contact-us/).

If you have it from previous installation, place `clientIds.php` next to the VagrantFile - this will be reused. Otherwise wait for client\_id and client\_secret, and put them into `frontend/ariademo/clientIds.php`after installation.

```php
<?php
// Unique client ID that lets ARIA know where your request is coming from
$client_id = 'xxx';
// keep this one on the server for security (used for generating access tokens)
$client_secret = 'yyy';
?>
```
{% endhint %}

Execute following in your command line:

```text
git clone https://github.com/h2020-westlife-eu/wp6-repository.git
cd wp6-repository
# (optionally unzip sp files from westlife sso and clientIds from ARIA) 
# unzip westlifespfiles.zip
vagrant up
```

If new sp-metadata.xml was generated, send it to westlife-aai@ics.muni.cz in order to enable authentication via West-Life AAI.

After successful finish. The VM is prepared, the port 8080 is automatically forwarded to VM's port 80. To connect to VM using SSH 

`vagrant ssh `

The backend DB is using randomly generated access credentials at `/etc/westlife/repository.key`

You may source this file by using `source /etc/westlife/repository.key`

You may restart the backend service by `service westlife-repository restart`

