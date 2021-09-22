# JWT Ondertekenen

Koppeltaal gebruikt JWT tokens op verschillende plekken. Voor de dev  guide zijn twee plekken van belang:

1. Voor het [ophalen van een access token](../toegang-tot-koppeltaal/smart-backend-service.md) die toegang geeft tot de Koppeltaal server.
2. Voor het uitvoeren van een [Koppeltaal launch](../../launchen/) middels [HTI](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md).

### Wat is een JWT?

JWT staat voor JSON Web Token. Een praktische uitleg over hoe een JWT werkt is [hier](https://jwt.io/introduction) te vinden. Een belangrijk stuk is:

> In its compact form, JSON Web Tokens consist of three parts separated by dots \(`.`\), which are:
>
> * Header
> * Payload
> * Signature
>
> Therefore, a JWT typically looks like the following.
>
> `xxxxx.yyyyy.zzzzz`

### Signen

Het ondertekenen van de JWT vindt plaats in de derde deel van de JWT: de signature. Koppeltaal gebruikt asymmetrische key pairs om de JWTs te ondertekenen. Het signature-deel wordt geëncrypt middels de private-key van de asymmetrische key pair. Het public-key deel wordt gepubliceerd onder de [JWKS](jwks-opzetten.md). Zo wordt bewezen dat een JWT daadwerkelijk van een partij komt die in het bezit is van de private-key. Het signen van de JWT kan het makkelijkst  uitgevoerd worden middels een JWT library voor de betreffende programmeertaal.

{% hint style="info" %}
De JWT debugger is een mooie plek om runtime te kijken wat de  inhoud  van de JWT is en hoe de token er dan uitziet. Let op dat het geselecteerde algoritme een RS-variant is  aangezien wij met key pairs werken en niet met shared secrets.
{% endhint %}

{% hint style="info" %}
RSA is ingewikkelder te implementeren dan bijv. HMAC algoritmen. Het is echter wel een stuk veiliger. Zo is er geen gedeelde secret. Ook is het middels JWKS snel mogelijk om keys in te roteren.
{% endhint %}

