# Create a Resource

{% hint style="info" %}
See the [FHIR documentation](https://www.hl7.org/fhir/r4/http.html#create) for more information.
{% endhint %}

### Business identifiers

Koppeltaal enforces setting business identifiers on `Resources` by making the `identifier` field mandatory. The main reason for this is so that a source system can consistently keep track of whether a Koppeltaal variant of their entity already exists. In addition, business identifiers can help when there are multiple source systems that need to know if a `Resource` already exists.

### Koppeltaal Profiles

The Koppeltaal server validates all `Resources` being created or updated. The server enforces that resources are sent in compliance with the [Koppeltaal profiles](https://simplifier.net/Koppeltaalv2.0/\~resources?category=Profile\&fhirVersion=R4\&sortBy=RankScore\_desc). Profiles are stored in FHIR as [`StructureDefinition`](https://www.hl7.org/FHIR/r4/structuredefinition.html) resources. To indicate that a resource has been created in compliance with a profile, the `Resource.meta.profiles` array must be filled. The value should always be filled with the canonical identifier of the profile. This can be found in [simplifier](https://simplifier.net/Koppeltaalv2.0/\~resources?category=Profile\&fhirVersion=R4\&sortBy=RankScore\_desc):

![Canonical identifier ophalen bij simplifier.net](<../../../.gitbook/assets/Screenshot 2021-12-09 at 10.42.16.png>)

For example:

```json
{
  "resourceType": "Subscription",
  "meta": {
    "profile": [
      "http://koppeltaal.nl/fhir/StructureDefinition/KT2Subscription"
    ]
  }
  ...
  }
}
```

### Conditional Create

{% hint style="warning" %}
The conditional create is still in the "trial use" phase. Thus, the status of this functionality has yet to be reviewed.
{% endhint %}

The FHIR specification describes [conditional creates](https://www.hl7.org/fhir/r4/http.html#ccreate). When a `Resource` is created, an `upsert` can be performed based on the business identifier. When multiple applications in a domain create the same type of `Resources`, it is important that there is clear agreement on which identifier system is used. The conditional create helps prevent duplicate resources being created at Koppeltaal.

{% swagger baseUrl="https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT" path="/<Resource>" method="post" summary="Conditional Create Request" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`application/fhir+json`

 OR 

`application/fhir+xml`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="If-None-Exist" type="string" required="true" %}
The business identifier, e.g:

`identifier=http://my-lab-system|123`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
Bearer token obtained from the Auth Server

\\

(see

[Connecting to Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="object" required="true" %}
Resource
{% endswagger-parameter %}

{% swagger-response status="200" description="The resource already existed. The POST was not processed." %}
```
```
{% endswagger-response %}

{% swagger-response status="201" description="Resource is created. The resource with resource-origin extension and id is returned." %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="The resource cannot be parsed or does not conform to the basic FHIR validation rules" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="Resource type is not supported" %}
```
```
{% endswagger-response %}

{% swagger-response status="412" description="More than one match found. The If-None-Exist is not selective enough." %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="Does not meet FHIR profiles or Koppeltaal business rules" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT" path="/<Resource>" method="post" summary="Create Request" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Content-type" required="true" %}
`application/fhir+json`

OR

`application/fhir+xml`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
Bearer token obtained from the Auth Server

\\

(see

[Connecting to Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="object" required="true" %}
Resource
{% endswagger-parameter %}

{% swagger-response status="201" description="Resource is created. The resource with resource-origin extension and id is returned." %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="The resource cannot be parsed or does not conform to the basic FHIR validation rules" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="Resource type is not supported" %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="Does not meet FHIR profiles or Koppeltaal business rules" %}
```
```
{% endswagger-response %}
{% endswagger %}

## Topics

[TOP-KT-002a - FHIR Resource Service interacties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125763/TOP-KT-002a+-+FHIR+Resource+Service+interacties)

[TOP-KT-005a - Rollen en rechten voor applicatie-instanties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123707/TOP-KT-005a+-+Rollen+en+rechten+voor+applicatie-instanties)

[TOP-KT-009 - Overzicht gebruikte FHIR Resources](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27071328/TOP-KT-009+-+Overzicht+gebruikte+FHIR+Resources)
