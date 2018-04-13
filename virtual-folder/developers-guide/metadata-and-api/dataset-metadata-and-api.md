# Dataset metadata and API

{% api-method method="get" host="https://\[virtual folder server\]" path="/metadataservice/dataset/{id}" %}
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
ID of the cake to get, for free of course.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{"Id"": 1,"Name":"dataset 1","Entries":[{"Id":1,"Type":"pdb_id","Name":"2hhd"}]}
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

