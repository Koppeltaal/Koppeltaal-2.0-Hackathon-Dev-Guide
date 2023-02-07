# Initiating a launch

{% hint style="warning" %}
It is still under discussion whether to rename the `token` param to `launch` and also to include an `iss` just like the SHOF launch. This way it does not matter whether an HTI or SHOF launch is sent out.
{% endhint %}

The launch can be initiated via a `<form>` and the `form-post-redirect` flow. Both HTI and SHOF are very similar, they just vary in parameter names:

### HTI

```html
<html>
<head>
</head>
<body onload="document.forms[0].submit();">
<form action="https://module.provider.eu/modules/x" method="post">
<input type="hidden" name="token" value="eyJhbGciO..."/>
</form>
</body>
</html>
```

{% swagger baseUrl="{{ActivityDefinition.endpoint}}" path="/" method="post" summary="HTI Launch " expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
application/x-www-form-urlencoded
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" required="true" %}
Signed JWT
{% endswagger-parameter %}

{% swagger-response status="302: Found" description="Redirects to the ActivityDefinition.endpoint value" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

### SHOF

```html
<html>
<head>
</head>
<body onload="document.forms[0].submit();">
<form action="https://module.provider.eu/modules/x" method="post">
<input type="hidden" name="launch" value="eyJhbGciO..."/>
<input type="hidden" name="iss" value="https%3A%2F%2Ffhir-server.koppeltaal.headease.nl%2Ffhir%2FDEFAULT"/>
</form>
</body>
</html>
```

{% swagger method="post" path="/" baseUrl="{{ActivityDefinition.endpoint}}" summary="SHOF launch" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
application/x-www-form-urlencoded
{% endswagger-parameter %}

{% swagger-parameter in="body" name="launch" required="true" %}
Signed JWT
{% endswagger-parameter %}

{% swagger-parameter in="body" name="iss" required="true" %}
The URL of the Koppeltaal server
{% endswagger-parameter %}
{% endswagger %}

