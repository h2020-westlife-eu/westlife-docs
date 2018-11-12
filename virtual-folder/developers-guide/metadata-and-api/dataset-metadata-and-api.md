# Dataset metadata and API

{% api-method method="get" host="https://\[virtual folder server\]" path="/virtualfolder/api/dataset/{id}" %}
{% api-method-summary %}
Get dataset
{% endapi-method-summary %}

{% api-method-description %}
Gets list of all datasets published by user or detail of dataset if "id" is specified.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" %}
ID of the dataset
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{"Id":6,"Owner":"vagrant","Name":"vagrant/filesystem/patient_data/data","Entries":[],"Metadata":"{}","Provenance":"document    \n    prefix virtualfolder <https://portal.west-life.eu/virtualfolder/>\n    prefix datafile <http://localhost:8081/webdav/vagrant/filesystem/patient_data/data/> \n\n    prefix westlife <https://about.west-life.eu/>\n    prefix thisvf <http://localhost:8081/virtualfolder/#/filemanager>\n    prefix user <>\n    entity (datafile:, [prov:label=\"data\", prov:type=\"document\"]) \n    \n    agent (user:vagrant, [ prov:type=\"prov:Person\" ]) \n    wasAttributedTo(datafile:, user:vagrant) \nendDocument"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a dataset specified
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

