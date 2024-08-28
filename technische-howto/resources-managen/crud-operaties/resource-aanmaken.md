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

## Conditional Create Request

<mark style="color:green;">`POST`</mark> `https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/<Resource>`

#### Headers

<table><thead><tr><th>Name</th><th width="225">Type</th><th>Description</th></tr></thead><tbody><tr><td>Content-Type<mark style="color:red;">*</mark></td><td>String</td><td><p><code>application/fhir+json</code></p><p>OR</p><p><code>application/fhir+xml</code></p></td></tr><tr><td>If-None-Exist<mark style="color:red;">*</mark></td><td>string</td><td><p>The business identifier, e.g:</p><p><code>identifier=http://my-lab-system|123</code></p></td></tr><tr><td>Authorization<mark style="color:red;">*</mark></td><td>string</td><td><p>Bearer token obtained from the Auth Server</p><p></p><p>(see <a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a>)</p></td></tr></tbody></table>

#### Request Body

| Name                               | Type   | Description |
| ---------------------------------- | ------ | ----------- |
| <mark style="color:red;">\*</mark> | object | Resource    |

{% tabs %}
{% tab title="200 " %}
The resource already existed. The POST was not processed.
{% endtab %}

{% tab title="201 " %}
Resource is created. The resource with resource-origin extension and id is returned.
{% endtab %}

{% tab title="400" %}
The resource cannot be parsed or does not conform to the basic FHIR validation rules
{% endtab %}

{% tab title="401" %}
Unauthenticated
{% endtab %}

{% tab title="403" %}
Unauthorized
{% endtab %}

{% tab title="404" %}
Resource type is not supported
{% endtab %}

{% tab title="412" %}
More than one match found. The If-None-Exist is not selective enough.
{% endtab %}

{% tab title="422" %}
Does not meet FHIR profiles or Koppeltaal business rules
{% endtab %}
{% endtabs %}

## Create Request

<mark style="color:green;">`POST`</mark> `https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/<Resource>`

#### Headers

| Name                                            | Type   | Description                                                                                                                                                     |
| ----------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Content-type<mark style="color:red;">\*</mark>  | String | <p><code>application/fhir+json</code></p><p>OR</p><p><code>application/fhir+xml</code></p>                                                                      |
| Authorization<mark style="color:red;">\*</mark> | string | <p>Bearer token obtained from the Auth Server</p><p>\</p><p>(see</p><p><a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a></p><p>)</p> |

#### Request Body

| Name                               | Type   | Description |
| ---------------------------------- | ------ | ----------- |
| <mark style="color:red;">\*</mark> | object | Resource    |

{% tabs %}
{% tab title="201" %}
Resource is created. The resource with resource-origin extension and id is returned.
{% endtab %}

{% tab title="400" %}
The resource cannot be parsed or does not conform to the basic FHIR validation rules
{% endtab %}

{% tab title="401" %}
Unauthenticated
{% endtab %}

{% tab title="403" %}
Unauthorized
{% endtab %}

{% tab title="404" %}
Resource type is not supported
{% endtab %}

{% tab title="422" %}
Does not meet FHIR profiles or Koppeltaal business rules
{% endtab %}
{% endtabs %}

## Topics

[TOP-KT-002a - FHIR Resource Service interacties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125763/TOP-KT-002a+-+FHIR+Resource+Service+interacties)

[TOP-KT-005a - Rollen en rechten voor applicatie-instanties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123707/TOP-KT-005a+-+Rollen+en+rechten+voor+applicatie-instanties)

[TOP-KT-009 - Overzicht gebruikte FHIR Resources](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27071328/TOP-KT-009+-+Overzicht+gebruikte+FHIR+Resources)
