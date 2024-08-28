---
description: Request all resources for a specific Resource-type
---

# Retrieve all Resources

{% hint style="info" %}
See the [FHIR documentation](https://www.hl7.org/fhir/r4/http.html#read) for more information
{% endhint %}

{% hint style="info" %}
Returned results can be filtered by [Search Narrowing](../../../domeinbeheer/rollen-beheren/search-narrowing.md).
{% endhint %}

## Retrieve all Resources

<mark style="color:blue;">`GET`</mark> `https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/<Resource>`

Get ALL for type .

#### Path Parameters

| Name                                       | Type   | Description                    |
| ------------------------------------------ | ------ | ------------------------------ |
| Resource<mark style="color:red;">\*</mark> | string | Resource-type (e.g. `Patient`) |

#### Headers

<table><thead><tr><th>Name</th><th width="236">Type</th><th>Description</th></tr></thead><tbody><tr><td>Authentication<mark style="color:red;">*</mark></td><td>string</td><td><p>Bearer token obtained from the Auth Server</p><p></p><p>(zie <a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a>)</p></td></tr></tbody></table>



{% tabs %}
{% tab title="200" %}
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
{% endtab %}

{% tab title="401" %}
Unauthenticated
{% endtab %}

{% tab title="403" %}
Unauthorized
{% endtab %}
{% endtabs %}

## Topics

[TOP-KT-002a - FHIR Resource Service interacties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125763/TOP-KT-002a+-+FHIR+Resource+Service+interacties)

[TOP-KT-005a - Rollen en rechten voor applicatie-instanties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123707/TOP-KT-005a+-+Rollen+en+rechten+voor+applicatie-instanties)

[TOP-KT-009 - Overzicht gebruikte FHIR Resources](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27071328/TOP-KT-009+-+Overzicht+gebruikte+FHIR+Resources)
