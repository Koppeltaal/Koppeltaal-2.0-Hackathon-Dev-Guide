---
description: Haalt één specifieke Resource op
---

# Resource Ophalen

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#read) voor meer informatie.
{% endhint %}

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>" method="get" summary="Specifieke  Resource Ophalen" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="string" %}
Resource.id to be  retrieved
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token verkregen via de Auth Server
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>/_history/<:versie>" method="get" summary="Specifieke  versie van een Resource ophalen" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" %}
Resource.id to be  retrieved
{% endswagger-parameter %}

{% swagger-parameter in="path" name="versie" type="string" %}
De versie
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="Versie bestaat niet" %}
```
```
{% endswagger-response %}
{% endswagger %}
