# HTI Launch Versturen

{% hint style="warning" %}
Het staat nog ter discussie om bij de HTI Launch de token als`launch` param mee te geven en om ook een `iss` mee te geven net als [SHOF Launch Versturen](shof-launch-versturen.md). Op deze manier maakt het niet uit of er een HTI of SHOF launch uitgestuurd wordt.
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

{% swagger baseUrl="{{ActivityDefinition.endpoint}}" path="" method="post" summary="HTI Launch " %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" %}
application/x-www-form-urlencoded
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" %}
Ondertekende JWT
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

