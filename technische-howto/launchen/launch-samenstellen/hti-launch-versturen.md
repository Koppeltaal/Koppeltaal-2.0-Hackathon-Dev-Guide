# HTI Launch  Versturen

{% hint style="warning" %}
Het staat nog ter discussie om bij de HTI Launch de token als `launch` param mee te geven en om ook een `iss` mee te geven net als [SHOF Launch Versturen](shof-launch-versturen.md). Op deze manier maakt het niet uit of er een HTI of SHOF launch uitgestuurd wordt.
{% endhint %}

Middels een `<form>` en de `form-post-redirect` flow kan de launch uitgevoerd worden. Bijv:

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

{% api-method method="post" host="{{ActivityDefinition.endpoint}}" path="" %}
{% api-method-summary %}
HTI Launch 
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Type" required=true %}
application/x-www-form-urlencoded
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-form-data-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
Ondertekende JWT
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



