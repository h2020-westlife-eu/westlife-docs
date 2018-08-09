# Import settings from another Virtual Folder

You may import settings from another deployment of virtual folder, e.g. import settings from your public virtual folder to the local one using "Import from public Virtual Folder". 

* Go to settings page "Virtual Folder" -&gt; "Setting" ![](https://github.com/h2020-westlife-eu/westlife-docs/tree/31f6e2b90206a4d8962f5f78ea55add47fab55cd/.gitbook/assets/settingsimport3.PNG)

![](../../../.gitbook/assets/importanimation3.gif)

* click "Import File Provider" button

![](../../../.gitbook/assets/import2.PNG)

* Enter URL of West-Life Virtual Folder instance and click "OK". Pop up window will appear.
* Select providers you would like to import and click "Import selected provider setting"

![](../../../.gitbook/assets/import3.PNG)

* The settings will appear in confirmation dialog, you may change alias of providers in case there is conflict with existing one.

![](../../../.gitbook/assets/import4.PNG)

* The settings are imported and available in your Virtual Folder instance.

A one-time RSA 2048 keys and AES encryption are used to transfer settings as they contain tokens or credentials to access your data. For technical details follow Developer's guide -&gt; Import Export Settings API

