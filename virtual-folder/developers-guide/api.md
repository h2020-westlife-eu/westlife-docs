# API

{% api-method method="get" host="https://\[virtual folder server\]" path="/metadataservice/files" %}
{% api-method-summary %}
Get Files
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
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "name": "Cake's name",
    "recipe": "Cake's recipe name",
    "cake": "Binary cake"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a cake matching this query.
{% endapi-method-response-example-description %}

```javascript
{
    "message": "Ain't no cake like that."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



