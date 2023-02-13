# Delete a Resource

### Do not use deletes

In Koppeltaal, we do not delete resources by default. As an alternative, we mark a resource as being end-of-life. This can be done with the following properties:

| `ActivityDefinition` | status | retired                                |
| -------------------- | ------ | -------------------------------------- |
| `Endpoint`           | status | off                                    |
| `Device`             | status | inactive                               |
| `Task`               | status | completed\|cancelled\|failed\|rejected |
| `Patient`            | active | false                                  |
| `Practitioner`       | active | false                                  |
| `RelatedPerson`      | active | false                                  |
| `CareTeam`           | status | inactive                               |
| `Subscription`       | status | off                                    |

### Logical deletes

{% hint style="info" %}
Keep in mind that the request below is normally NOT used. Please read the section above.
{% endhint %}

When an application does get the permission to execute a `DELETE` request, the resource will be marked as deleted. When a client requests this resource, the server will respond with a `410 Gone`. However, applications will still be allowed to request older versions via the [vread](resource-ophalen.md#retrieve-specific-version-of-a-resource-vread) request.

{% swagger method="delete" path="/<Resource>/<id>" baseUrl="https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT" summary="Logical deletion of a specific resource instance" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="<Resource>" required="true" %}
The resource type, e.g: 

`Patient`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="<id>" required="true" %}
The instance id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Delete successful, returns an OperationOutcome" %}
```javascript
{
  "resourceType": "OperationOutcome",
  "text": {
    "status": "generated",
    "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\"><h1>Operation Outcome</h1><table border=\"0\"><tr><td style=\"font-weight: bold;\">INFORMATION</td><td>[]</td><td>Successfully deleted 1 resource(s). Took 22ms.</td></tr></table></div>"
  },
  "issue": [
    {
      "severity": "information",
      "code": "informational",
      "details": {
        "coding": [
          {
            "system": "https://hapifhir.io/fhir/CodeSystem/hapi-fhir-storage-response-code",
            "code": "SUCCESSFUL_DELETE",
            "display": "Delete succeeded."
          }
        ]
      },
      "diagnostics": "Successfully deleted 1 resource(s). Took 22ms."
    }
  ]
}
```
{% endswagger-response %}

{% swagger-response status="204: No Content" description="Delete successful, no OperationOutcome" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Resource not found" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

### Right to be forgotten

In scenarios where users are able to use their right to be forgotten, the deletion of the data should always be done via the domain admin. Some solutions might require a manual delete, and certain solutions might support a `DELETE` request with the `$expunge` operator.
