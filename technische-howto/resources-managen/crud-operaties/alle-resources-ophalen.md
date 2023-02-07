---
description: Request all resources for a specific Resource-type
---

# Retrieving all Resources

{% hint style="info" %}
See the [FHIR documentation](https://www.hl7.org/fhir/http.html#read) for more information
{% endhint %}

{% hint style="info" %}
Returned results can be filtered by [Search Narrowing](../../../domeinbeheer/rollen-beheren/search-narrowing.md).
{% endhint %}

{% swagger baseUrl="https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT" path="/<Resource>" method="get" summary="Retrieve all Resources" %}
{% swagger-description %}
Get ALL for type <Resource>. 
{% endswagger-description %}

{% swagger-parameter in="path" name="Resource" type="string" required="true" %}
Resource-type (e.g. 

`Patient`

)
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Bearer token obtained from the Auth Server 

\


(zie 

[Connectie maken met Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
{% endswagger-parameter %}

{% swagger-response status="200" description="Successful response that contains 2 Patient resources. Patient information has been taken out" %}
```json
{
  "resourceType": "Bundle",
  "id": "f0cfec85-0d26-44f9-a222-a478451d4bf9",
  "meta": {
    "lastUpdated": "2023-02-07T08:16:54.158+00:00"
  },
  "type": "searchset",
  "link": [
    {
      "relation": "self",
      "url": "https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/Patient"
    },
    {
      "relation": "next",
      "url": "https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT?_getpages=f0cfec85-0d26-44f9-a222-a478451d4bf9&_getpagesoffset=40&_count=40&_pretty=true&_bundletype=searchset"
    }
  ],
  "entry": [
    {
      "fullUrl": "https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/Patient/13befac2-de72-4926-99e8-24d0adb1fd62",
      "resource": {
        "resourceType": "Patient",
        "id": "13befac2-de72-4926-99e8-24d0adb1fd62",
        ...
      },
      "search": {
        "mode": "match"
      }
    },
    {
      "fullUrl": "https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/Patient/d20b53b0-5b35-493c-9cbd-40673d5ce22e",
      "resource": {
        "resourceType": "Patient",
        "id": "d20b53b0-5b35-493c-9cbd-40673d5ce22e",
        ...
      },
      "search": {
        "mode": "match"
      }
    }
  ]
}

```
{% endswagger-response %}
{% endswagger %}
