---
description: The launch steps on this page are the same for HTI as SHOF
---

# Initiating a launch

The launch can be initiated via a `<form>` and the `form-post-redirect` flow. Both HTI and SHOF are similar while initiating a launch.

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

{% swagger method="post" path="/" baseUrl="{{ActivityDefinition.endpoint}}" summary="Launch" expanded="true" %}
{% swagger-description %}
This request initializes the launch. 
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="launch" required="true" %}


[Signed JWT](./)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="iss" required="true" %}
The URL of the Koppeltaal server
{% endswagger-parameter %}
{% endswagger %}

