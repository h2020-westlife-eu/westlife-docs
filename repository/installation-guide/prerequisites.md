# Prerequisites

External registration is needed with West-Life SSO and ARIA for integrating single sign on and project proposal import. It allows to login into Visiting Scientist space and allows import project proposals from Instruct's database at [https://www.structuralbiology.eu](https://www.structuralbiology.eu)

Demo registration package can be provided upon request.

{% hint style="warning" %}
In order to integrate with West-Life SSO, you need your `sp-metadata,idp-metadata,sp_key`and`sp_cert` files.

If you have it from previous installation, then put it next to the VagrantFile - these will be reused.

If you don't have, Edit the bootstrap.sh file and change values of variables to your hostname and registered identification:

`SP_IDENTIFICATION=http://[your public domain of repository]`

`SP_ENDPOINT=http://[your public domain of repository]/mellon`

After first `vagrant up` or `bootstrap.sh`, find the generated file `sp-metadata.xml` and register it with West-Life SSO - send it to westlife-aai@ics.muni.cz
{% endhint %}

{% hint style="warning" %}
In order to have working ARIA integration, you need your URL of repository registered with Instruct. If you have it from previous installation, place `clientIds.php` next to the VagrantFile - this will be reused. Otherwise send a request for client\_id and client\_secret for the URL in the form `http://[your public domain of repository]/repository` to [https://www.structuralbiology.eu/contact-us/](https://www.structuralbiology.eu/contact-us/). You can continue with installation `vagrant up`. When you receive `client\_id` and `client\_secret`, create `frontend/ariademo/clientIds.php`.

```php
<?php
// Unique client ID that lets ARIA know where your request is coming from
$client_id = 'xxx';
// keep this one on the server for security (used for generating access tokens)
$client_secret = 'yyy';
?>
```
{% endhint %}

