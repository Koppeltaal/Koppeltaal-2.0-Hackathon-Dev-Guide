# Performing a HTI Launch



{% hint style="warning" %}
It is still under discussion whether to rename the `token` param to `launch` and also to include an `iss` just like [Performing a SHOF launch](shof-launch-versturen.md). This way it does not matter whether an HTI or SHOF launch is sent out.
{% endhint %}

The launch can be initiated via a `<form>` and the `form-post-redirect` flow, e.g:

```markup
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

{% swagger baseUrl="{{ActivityDefinition.endpoint}}" path="/" method="post" summary="HTI Launch " %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
application/x-www-form-urlencoded
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" required="true" %}
Ondertekende JWT
{% endswagger-parameter %}

{% swagger-response status="302: Found" description="Redirects to the ActivityDefinition.endpoint value" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

