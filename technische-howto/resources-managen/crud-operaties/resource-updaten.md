# Update a Resource

{% hint style="info" %}
See the [FHIR documentation](https://www.hl7.org/fhir/r4/http.html#update) for more information.
{% endhint %}

### Concurrency

To avoid overwriting data, an application must always indicate which version of a `Resource` the update is based on. This is done using the `If-Match` header. If this header is missing, the Koppeltaal server will reject the request. If the update is not based on the latest version, the server will respond with a `409 Conflict` or a `412 Precondition Failed`.

The `If-Match` value must match the latest `ETag` value. The `ETag` value is provided via a response header sent by the Koppeltaal server after a [`Create`](resource-aanmaken.md), [`Update`](resource-updaten.md) or [`Get`](resource-ophalen.md#retrieve-specific-resource).

## Update a complete resource

<mark style="color:orange;">`PUT`</mark> `https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/<Resource>/<:id>`

Note: the

`id`

property has to be set in the body as well

#### Path Parameters

| Name                                 | Type   | Description                                                |
| ------------------------------------ | ------ | ---------------------------------------------------------- |
| id<mark style="color:red;">\*</mark> | string | <p>The "logical id" of the</p><p><code>Resource</code></p> |

#### Headers

<table><thead><tr><th>Name</th><th width="217">Type</th><th>Description</th></tr></thead><tbody><tr><td>If-Match<mark style="color:red;">*</mark></td><td>string</td><td><p>A</p><p><a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match">"weak" ETag</a></p><p>to the version the update is based on, e.g: W/"3"</p></td></tr><tr><td>Authorization<mark style="color:red;">*</mark></td><td>string</td><td><p>Bearer token obtained from the Auth Server</p><p></p><p>(see <a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a>)</p></td></tr></tbody></table>

#### Request Body

| Name                               | Type   | Description                            |
| ---------------------------------- | ------ | -------------------------------------- |
| <mark style="color:red;">\*</mark> | string | <p>The</p><p><code>Resource</code></p> |

{% tabs %}
{% tab title="200" %}
Resource is modified. The resource with resource-origin extension and logical id is returned
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
Resource type not supported, or not a FHIR end-point
{% endtab %}

{% tab title="405" %}
The Resource did not exist prior to the update, and the server does not allow client defined ids
{% endtab %}

{% tab title="409" %}
Version conflict, update is based on an old version
{% endtab %}

{% tab title="412" %}
Precondition Failed Version conflict, update is based on an old version
{% endtab %}

{% tab title="422" %}
Does not meet FHIR profiles or Koppeltaal business rules
{% endtab %}
{% endtabs %}

### Delen van een Resource Updaten

{% hint style="warning" %}
PATCH requests are optional. See the [`Conformance`](../../koppeltaal-server-metadata-opvragen.md#capabilitystatement) to find out if the server supports this.
{% endhint %}

To update a `Resource` via a small payload, the Koppeltaal server may support `PATCH` requests. The payload of the `PATCH` must be one of the following:

1. A [JSON Patch](https://datatracker.ietf.org/doc/html/rfc6902) (Content-Type application/json-patch+json).
2. An [XML Patch](https://datatracker.ietf.org/doc/html/rfc5261) (Content-Type application/xml-patch+xml)
3. A [FHIRPath Patch](https://www.hl7.org/fhir/r4/fhirpatch.html) parameters Resource (Content-Type [FHIR Content Type](https://www.hl7.org/fhir/r4/http.html#mime-type)).

This is what the payload looks like from a JSON Patch to update the status of a `Task`

```
[{
    "op": "replace",
    "path": "/status",
    "value": "completed"
}]
```

More examples of patches can be downloaded [here](https://www.hl7.org/fhir/r4/test-cases.zip).

## Patch a Resource

<mark style="color:purple;">`PATCH`</mark> `https://hapi-fhir-server.koppeltaal.headease.nl/fhir/<Resource>/<:id>`

As an alternative to updating an entire resource, clients can perform a patch operation. This can be useful when a client is seeking to minimize its bandwidth utilization.

#### Path Parameters

| Name                               | Type   | Description                                                |
| ---------------------------------- | ------ | ---------------------------------------------------------- |
| <mark style="color:red;">\*</mark> | String | <p>The "logical id" of the</p><p><code>Resource</code></p> |

#### Headers

<table><thead><tr><th>Name</th><th width="211">Type</th><th>Description</th></tr></thead><tbody><tr><td>If-Match<mark style="color:red;">*</mark></td><td>string</td><td><p>A</p><p><a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match">"weak" ETag</a></p><p>to the version the update is based on, e.g: W/"3"</p></td></tr><tr><td>Authorization<mark style="color:red;">*</mark></td><td>string</td><td><p>Bearer token obtained from the Auth Server</p><p></p><p>(see <a href="../../connectie-maken-met-koppeltaal/">Connecting to Koppeltaal</a>)</p></td></tr></tbody></table>

#### Request Body

| Name                               | Type   | Description |
| ---------------------------------- | ------ | ----------- |
| <mark style="color:red;">\*</mark> | object | The Patch   |

{% tabs %}
{% tab title="200" %}
Patch is applied. The complete Resource will be returned
{% endtab %}

{% tab title="401" %}
Unauthenticated
{% endtab %}

{% tab title="403" %}
Unauthorized
{% endtab %}

{% tab title="404" %}
```
Not Found Resource type not supported, or not a FHIR end-point
```
{% endtab %}

{% tab title="405" %}
```
Method Not Allowed 
```
{% endtab %}

{% tab title="412" %}
```
Precondition Failed Version conflict, update is based on an old version
```
{% endtab %}
{% endtabs %}

## Topics

[TOP-KT-002a - FHIR Resource Service interacties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125763/TOP-KT-002a+-+FHIR+Resource+Service+interacties)

[TOP-KT-005a - Rollen en rechten voor applicatie-instanties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123707/TOP-KT-005a+-+Rollen+en+rechten+voor+applicatie-instanties)

[TOP-KT-009 - Overzicht gebruikte FHIR Resources](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27071328/TOP-KT-009+-+Overzicht+gebruikte+FHIR+Resources)
