# JWKS Opzetten

Om op een veilige manier een ondertekende JWT te kunnen valideren wordt geadviseerd om gebruik te maken van [JSON Web Key Set \(JWKS\)](https://auth0.com/docs/security/tokens/json-web-tokens/json-web-key-sets). 

De applicatie moet er voor zorgen dat de gegenereerde key pair vertaald wordt naar [JSON Web Key](https://datatracker.ietf.org/doc/html/rfc7517)   formaat. Vervolgens worden dan één of meer JWK objecten aangeboden onder het JWKS endpoint: `https://YOUR_DOMAIN/.well-known/jwks.json` .

Omdat de public keys nu beschikbaar zijn onder een vaste URL, kan met gemak een key worden ingetrokken of geroteerd.

{% hint style="info" %}
Heel veel programmeertalen hebben libraries tot hun beschikking om simpel een RSA key in PEM formaat \(semi\)automatisch aan te bieden als JWKS endpoint. Kijk goed wat er beschikbaar is voordat je dit zelf implementeert!
{% endhint %}



