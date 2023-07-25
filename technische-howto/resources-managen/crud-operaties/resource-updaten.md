# Update a Resource

{% hint style="info" %}
See the [FHIR documentation](https://www.hl7.org/fhir/r4/http.html#update) for more information.
{% endhint %}

### Concurrency

To avoid overwriting data, an application must always indicate which version of a `Resource` the update is based on. This is done using the `If-Match` header. If this header is missing, the Koppeltaal server will reject the request. If the update is not based on the latest version, the server will respond with a `409 Conflict` or a `412 Precondition Failed`.

The `If-Match` value must match the latest `ETag` value. The `ETag` value is provided via a response header sent by the Koppeltaal server after a [`Create`](resource-aanmaken.md), [`Update`](resource-updaten.md) or [`Get`](resource-ophalen.md#retrieve-specific-resource).

{% swagger baseUrl="https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT" path="/<Resource>/<:id>" method="put" summary="Update a complete resource" expanded="true" %}
{% swagger-description %}
Note: the

`id`

property has to be set in the body as well
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
The "logical id" of the

`Resource`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="If-Match" type="string" required="true" %}
A

["weak" ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match)

to the version the update is based on, e.g: W/"3"
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
Bearer token obtained from the Auth Server

\\

(see

[Connecting to Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="string" required="true" %}
The

`Resource`
{% endswagger-parameter %}

{% swagger-response status="200" description="Resource is modified. The resource with resource-origin extension and logical id is returned" %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="The resource cannot be parsed or does not conform to the basic FHIR validation rules" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="Resource type not supported, or not a FHIR end-point" %}
```
```
{% endswagger-response %}

{% swagger-response status="405" description="The Resource did not exist prior to the update, and the server does not allow client defined ids" %}
```
```
{% endswagger-response %}

{% swagger-response status="409" description="Version conflict, update is based on an old version" %}
```
```
{% endswagger-response %}

{% swagger-response status="412: Precondition Failed" description="Version conflict, update is based on an old version" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="422" description="Does not meet FHIR profiles or Koppeltaal business rules" %}
```
```
{% endswagger-response %}
{% endswagger %}

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

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>" method="patch" summary="Patch a Resource" expanded="true" %}
{% swagger-description %}
As an alternative to updating an entire resource, clients can perform a patch operation. This can be useful when a client is seeking to minimize its bandwidth utilization.
{% endswagger-description %}

{% swagger-parameter in="path" required="true" %}
The "logical id" of the

`Resource`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="If-Match" type="string" required="true" %}
A

["weak" ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match)

to the version the update is based on, e.g: W/"3"
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
Bearer token obtained from the Auth Server

\\

(see

[Connecting to Koppeltaal](../../connectie-maken-met-koppeltaal/)

)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="object" required="true" %}
The Patch
{% endswagger-parameter %}

{% swagger-response status="200" description="Patch is applied. The complete Resource will be returned" %}
```
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Resource type not supported, or not a FHIR end-point" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="405: Method Not Allowed" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="412: Precondition Failed" description="Version conflict, update is based on an old version" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

## Topics

[TOP-KT-002a - FHIR Resource Service interacties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125763/TOP-KT-002a+-+FHIR+Resource+Service+interacties)

[TOP-KT-005a - Rollen en rechten voor applicatie-instanties](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123707/TOP-KT-005a+-+Rollen+en+rechten+voor+applicatie-instanties)

[TOP-KT-009 - Overzicht gebruikte FHIR Resources](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27071328/TOP-KT-009+-+Overzicht+gebruikte+FHIR+Resources)
