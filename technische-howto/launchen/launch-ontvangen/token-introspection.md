# Token Introspection

Wanneer een applicatie een HTI launch ontvangt, zal deze een [JWT token bevatten](../launch-samenstellen/). Om er voor te zorgen dat niet elke applicatie alle security logica moet inbouwen om dit token te controleren, biedt Koppeltaal [Token Introspection](https://datatracker.ietf.org/doc/html/rfc7662) aan op de authenticatie server. Zo hoeft de applicatie niet meer zelf de [JWT te verifiëren](./#jwt-verifieren).

Wanneer de Token Introspection een token goedkeurt, zal de uitgepakte body van de JWT token teruggegeven worden. Indien er een `200` response code wordt teruggegeven, moet de applicatie ALTIJD verifiëren dat het `active` attribuut in de response `true` is.&#x20;

{% hint style="warning" %}
Momenteel vereist de introspection endpoint nog geen `Authorization` header. Dit wordt in de toekomstige versie wel verplicht. Deze waarde kan gevuld worden met [hetzelfde token](../../connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md) waarmee gecommuniceerd wordt met de Koppeltaal Server.
{% endhint %}

{% swagger method="post" path="/oauth2/introspect" baseUrl="{AUTH_SERVER_URL}" summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="client_assertion" required="true" %}
JWT token from launch
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Accept" required="true" %}
`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion_type" required="true" %}
Altijd: 

`urn:ietf:params:oauth:client-assertion-type:jwt-bearer`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
De "smart backend" access token die ook wordt gebruikt wordt om te communiceren met de Koppeltaal Server.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="De token is geïntrospect. Het active veld geeft weer of deze valide is" %}
{% tabs %}
{% tab title="Valid Token" %}
```json
{
  "active": true,
  "client_id": "l238j323ds-23ij4",
  "username": "jdoe",
  "scope": "read write dolphin",
  "sub": "Z5O3upPC88QrAjx00dis",
  "aud": "https://protected.example.net/resource",
  "iss": "https://server.example.com/",
  "exp": 1419356238,
  "iat": 1419350238,
  "extension_field": "twenty-seven"
}
```
{% endtab %}

{% tab title="Invalid Token" %}
```json
{
  "active": false
}
```
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Let op: Not niet geïmplementeerd. Later wordt de Authorization header verplicht. Wanneer de auth server deze afkeurt, zal dit de response worden." %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

