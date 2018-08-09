# Import Export Settings API

This section describes technical implementation of Importing Setting from one Virtual Folder to another. For user's guide follow [Import settings from another Virtual Folder](../users-guide/settings/import-settings-from-another-virtual-folder.md). 

Because user's credentials are transfered via network, the settings are encrypted. Virtual Folder API generates asymetric 2048 bytes long RSA key and random symetric AES key for each transfer.

The following workflow will export settings from Virtual Folder Exported \(VFE\) instance \(at **https://portal.west-life.eu/virtualfolder**\) into Virtual Folder Import \(VFI\) instance at **http://localhost:8081/virtualfolder**. During import process it's possible to rename imported aliases:

* To get public key from VFI instance, send POST request to VFI `api/settings` . 

```http
POST http://localhost:8081/virtualfolder/api/settings
```

* The public key is returned as plain text in response body:  

  ```markup
  <RSAKeyValue>
    <Modulus>pUPMWrEajQ2...</Modulus>
    <Exponent>AQAB</Exponent>
  </RSAKeyValue>
  ```

* To obtain selected file providers from VFE instance, put the names of aliases \(e.g. dropbox and experiment\_pcloud\)  into `SelectedAliases` query parameter and VFI public key into `PublicKey` query parameter and send it as GET request to VFE api/settings

```http
GET https://portal.west-life.eu/virtualfolder/api/settings?PublicKey=
<RSAKeyValue><Modulus>pUPMWrEajQ2...</Modulus>
<Exponent>AQAB</Exponent></RSAKeyValue>
&SelectedAliases=dropbox;experiment_pcloud
```

* The response body from VFE contain encrypted settings using the RSA public key and AES encryption

```markup
PBy4F6DjCDmUg1h3HxIpR2I6ABNl6kEDoVuN043C4f9GGFjmSd33tVZxWeC...
```

* To import the encrypted settings into VFI, attach public key, encrypted settings, old name aliases from VFE as parameter `ConflictedAliases` and new names of aliases in the same order which will appear in VFI as parameter `NewNameAliases` and send it as PUT request with e.g. the following json body \(this example will import VFE b2drop as VFI b2drop\_test\)

```javascript
PUT http://localhost:8081/virtualfolder/api/settings
{
"PublicKey":"<RSAKeyValue><Modulus>pUPMWrEajQ2...",
"EncryptedSettings":"PBy4F6DjCDmUg1h3GGFjmSd33tVZxWeC...",
"ConflictedAliases":"b2drop",
"NewNameAliases":"b2drop_test"
}
```

* The PUT request will return array of all registered aliases, where a new alias should appear at the end.

```javascript
[{"Id":76,"alias":"filesystem","type":"FileSystem"},
{"Id":77,"alias":"b2drop_test","type":"B2Drop","username":"...","output":"..."}]
```



