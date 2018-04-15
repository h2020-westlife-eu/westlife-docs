# File metadata and API

{% api-method method="get" host="https://\[virtual folder server\]" path="/metadataservice/files" %}
{% api-method-summary %}
Get Providers
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get root directory structure consiting of registered storage providers. 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Root directory structure succesfully retrieved.
{% endapi-method-response-example-description %}

```javascript
[{
    "Id": "2",
    "alias": "b2drop",
    "type": "B2Drop",
    "username": "myusername",
    "output": "debugging output in case of error"
}]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Not logged. A session needs to be authorized by loging - valid cookie is set by previous "login" dialog process.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://\[virtual folder server\]" path="/metadataservice/files/\[provider\]" %}
{% api-method-summary %}
Get Files from provider
{% endapi-method-summary %}

{% api-method-description %}
Get files from selected provider
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="provider" type="string" required=true %}
provider name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[{"name":"directory1",
  "attributes":16,
  "size":0,
  "date":"/Date(1488898207000+0000)/",
  "path":"",
  "filetype":15,
  "webdavuri":"/webdav/tomas.kulhanek@stfc.ac.uk/b2drop/Physiolibrary.models",
  "publicwebdavuri":""}
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://\[virtual folder server\]" path="/metadataservice/dataset/{id}" %}
{% api-method-summary %}
Get dataset
{% endapi-method-summary %}

{% api-method-description %}
Get datasets registered within user account
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="number" %}
id of existing dataset
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{"Id"": 1,"Name":"dataset 1","Entries":[{"Id":1,"Type":"pdb_id","Name":"2hhd"}]}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}
