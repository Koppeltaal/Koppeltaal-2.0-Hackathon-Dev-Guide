---
description: Haalt één specifieke Resource op
---

# Resource Ophalen

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#read) voor meer informatie.
{% endhint %}

{% api-method method="get" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<id>" %}
{% api-method-summary %}
Specifieke  Resource Ophalen
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Resource.id to be  retrieved
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token verkregen via de Auth Server
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

