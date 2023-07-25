# JWKS setup

To securely validate a signed JWT, it is recommended to use [JSON Web Key Set (JWKS)](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-key-sets).

The application must ensure that the generated key pair is translated into [JSON Web Key (JWK)](https://datatracker.ietf.org/doc/html/rfc7517) format. One or more JWK objects are then offered under a JWKS endpoint: `https://YOUR_DOMAIN/path-to/jwks.json`.

Because the public keys are now available under a fixed URL, a key can be revoked or rotated with ease.

{% hint style="info" %}
A lot of programming languages have libraries available to simply (semi)automatically offer an RSA key in PEM format as a JWKS endpoint. Look carefully at what is available before implementing this yourself!
{% endhint %}

{% hint style="info" %}
&#x20;The use of a fixed path such as `https://YOUR_DOMAIN/.well-known/jwks.json` is discouraged to make hostile domain takeovers more complicated.
{% endhint %}

## Topics

[TOP-KT-020 - Uitwisseling publieke sleutels](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27126636/TOP-KT-020+-+Uitwisseling+publieke+sleutels)
