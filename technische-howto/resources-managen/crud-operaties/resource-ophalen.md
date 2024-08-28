---
description: Request a specific instance of a Resource
---

# Retrieve specific Resource

{% hint style="info" %}
See the [FHIR documentation](https://www.hl7.org/fhir/r4/http.html#read) for more information.
{% endhint %}

## Retrieve specific Resource

<mark style="color:blue;">`GET`</mark> `https://hapi-fhir-server.koppeltaal.headease.nl/fhir/<Resource>/<:id>`

#### Path Parameters

| Name                                 | Type   | Description                 |
| ------------------------------------ | ------ | --------------------------- |
| id<mark style="color:red;">\*</mark> | string | Resource.id to be retrieved |

#### Headers

| Name                                             | Type   | Description                                                                                                                                       |
| ------------------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authentication<mark style="color:red;">\*</mark> | string | <p>Bearer token obtained from the Auth Server (see</p><p><a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a></p><p>)</p> |

{% tabs %}
{% tab title="200 " %}
```json
{
    "resourceType": "Patient",
    "id": "13befac2-de72-4926-99e8-24d0adb1fd62",
    "meta": {
        "versionId": "1",
        "lastUpdated": "2023-01-17T08:52:04.997+00:00",
        "source": "urn:uuid:c712f204-abe7-4ec1-bad0-6dc3dfa1afd8#26be13a5038c6a77",
        "profile": [
            "http://koppeltaal.nl/fhir/StructureDefinition/KT2Patient"
        ]
    },
    "text": {
        "status": "generated",
        "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\"><div class=\"hapiHeaderText\">null <b>SUITE </b></div><table class=\"hapiPropertyTable\"><tbody><tr><td>Identifier</td><td>6cc2bea5</td></tr><tr><td>Date of birth</td><td><span>17 January 2023</span></td></tr></tbody></table></div>"
    },
    "extension": [
        {
            "url": "http://koppeltaal.nl/fhir/StructureDefinition/resource-origin",
            "valueReference": {
                "reference": "Device/9f3ac641-bf1a-4f62-82b2-9825f80e6847",
                "type": "Device"
            }
        }
    ],
    "identifier": [
        {
            "system": "https://koppeltaal.nl/identifier/TestSuiteIdentifier",
            "value": "6cc2bea5"
        }
    ],
    "active": true,
    "name": [
        {
            "use": "official",
            "family": "Suite",
            "given": [
                null
            ],
            "_given": [
                {
                    "extension": [
                        {
                            "url": "http://hl7.org/fhir/StructureDefinition/iso21090-EN-qualifier",
                            "valueCode": "IN"
                        }
                    ]
                }
            ]
        }
    ],
    "gender": "male",
    "birthDate": "2023-01-17"
}
```
{% endtab %}

{% tab title="401" %}
Unauthenticated
{% endtab %}

{% tab title="403" %}
Unauthorized
{% endtab %}

{% tab title="404" %}
```
Not Found Resource with id does not exist
```
{% endtab %}

{% tab title="410" %}
```
Gone Resource has been deleted
```
{% endtab %}
{% endtabs %}

## Retrieve specific version of a Resource (vread)&#x20;

<mark style="color:blue;">`GET`</mark> `https://hapi-fhir-server.koppeltaal.headease.nl/fhir/<Resource>/<:id>/_history/<:versie>`

#### Path Parameters

| Name                                     | Type    | Description                 |
| ---------------------------------------- | ------- | --------------------------- |
| id<mark style="color:red;">\*</mark>     | String  | Resource.id to be retrieved |
| versie<mark style="color:red;">\*</mark> | Integer | The version                 |

#### Headers

<table><thead><tr><th>Name</th><th width="221">Type</th><th>Description</th></tr></thead><tbody><tr><td>Authorization<mark style="color:red;">*</mark></td><td>String</td><td><p>Bearer token verkregen via de Auth Server</p><p></p><p>(zie <a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a> )</p></td></tr></tbody></table>

{% tabs %}
{% tab title="200" %}
```json
{
    "resourceType": "Patient",
    "id": "13befac2-de72-4926-99e8-24d0adb1fd62",
    "meta": {
        "versionId": "1",
        "lastUpdated": "2023-01-17T08:52:04.997+00:00",
        "source": "urn:uuid:c712f204-abe7-4ec1-bad0-6dc3dfa1afd8#26be13a5038c6a77",
        "profile": [
            "http://koppeltaal.nl/fhir/StructureDefinition/KT2Patient"
        ]
    },
    "text": {
        "status": "generated",
        "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\"><div class=\"hapiHeaderText\">null <b>SUITE </b></div><table class=\"hapiPropertyTable\"><tbody><tr><td>Identifier</td><td>6cc2bea5</td></tr><tr><td>Date of birth</td><td><span>17 January 2023</span></td></tr></tbody></table></div>"
    },
    "extension": [
        {
            "url": "http://koppeltaal.nl/fhir/StructureDefinition/resource-origin",
            "valueReference": {
                "reference": "Device/9f3ac641-bf1a-4f62-82b2-9825f80e6847",
                "type": "Device"
            }
        }
    ],
    "identifier": [
        {
            "system": "https://koppeltaal.nl/identifier/TestSuiteIdentifier",
            "value": "6cc2bea5"
        }
    ],
    "active": true,
    "name": [
        {
            "use": "official",
            "family": "Suite",
            "given": [
                null
            ],
            "_given": [
                {
                    "extension": [
                        {
                            "url": "http://hl7.org/fhir/StructureDefinition/iso21090-EN-qualifier",
                            "valueCode": "IN"
                        }
                    ]
                }
            ]
        }
    ],
    "gender": "male",
    "birthDate": "2023-01-17"
}
```
{% endtab %}

{% tab title="401" %}
Unauthenticated
{% endtab %}

{% tab title="403" %}
Unauthorized
{% endtab %}

{% tab title="404" %}
Resource with id does not exist
{% endtab %}

{% tab title="410 " %}
Gone Resource has been deleted
{% endtab %}
{% endtabs %}

## Topics

[TOP-KT-002a - FHIR Resource Service interacties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125763/TOP-KT-002a+-+FHIR+Resource+Service+interacties)

[TOP-KT-003 - Logische ID, bedrijfsidentifier, referenties en referentie integriteit](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27066395/TOP-KT-003+-+Logische+ID+bedrijfsidentifier+referenties+en+referentie+integriteit)

[TOP-KT-005a - Rollen en rechten voor applicatie-instanties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123707/TOP-KT-005a+-+Rollen+en+rechten+voor+applicatie-instanties)

[TOP-KT-009 - Overzicht gebruikte FHIR Resources](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27071328/TOP-KT-009+-+Overzicht+gebruikte+FHIR+Resources)
