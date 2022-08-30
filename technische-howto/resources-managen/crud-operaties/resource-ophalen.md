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

{% swagger-parameter in="path" name="id" type="string" required="true" %}
Resource.id to be  retrieved
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Bearer token verkregen via de Auth Server 

\


(zie 

[Connectie maken met Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>/_history/<:versie>" method="get" summary="Specifieke  versie van een Resource ophalen" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" required="true" %}
Resource.id to be  retrieved
{% endswagger-parameter %}

{% swagger-parameter in="path" name="versie" type="string" required="true" %}
De versie
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
Bearer token verkregen via de Auth Server 

\


(zie 

[Connectie maken met Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
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
