# CRUD Operaties

Primair overzicht van handige RESTful calls. De  kan vervangen worden met Resources van de [FHIR Resource list](https://www.hl7.org/fhir/resourcelist.html).

{% api-method method="get" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" %}
{% api-method-summary %}
Alle Resources ophalen
{% endapi-method-summary %}

{% api-method-description %}
Get ALL voor type &lt;Resource&gt;. 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Resource" type="string" required=true %}
Resource van de resource list
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token verkregen via de Auth Server   
\(zie Connectie maken met Koppeltaal\)
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

{% api-method method="get" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<id>" %}
{% api-method-summary %}
Specifieke Resource ophalen
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



