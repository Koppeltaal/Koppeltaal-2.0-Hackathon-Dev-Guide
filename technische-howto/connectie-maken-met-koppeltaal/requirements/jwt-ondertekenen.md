# Signing the JWT

Koppeltaal uses JWTs in several places. For the dev guide, two places are good to know about:

1. For [retrieving an access token](../toegang-tot-koppeltaal.md) that provides access to the Koppeltaal server.
2. For performing a [Koppeltaal launch](../../launchen/) using [HTI](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md) or [SMART HTI](../../launchen/smart-hti-on-fhir.md).

### What is a JWT?

JWT stands for JSON Web Token. A practical explanation of how a JWT works can be found [here](https://jwt.io/introduction). An important piece is:

> In its compact form, JSON Web Tokens consist of three parts separated by dots (`.`), which are:
>
> * Header
> * Payload
> * Signature
>
> Therefore, a JWT typically looks like the following.
>
> `xxxxx.yyyyy.zzzzz`

### Signing

The signing of the JWT takes place in the third part of the JWT: the signature (`zzzzz` in the example above). Koppeltaal uses asymmetric key pairs to sign the JWTs. The signature part is encrypted using the private key of the asymmetric key pair. The public-key part is published at the [JWKS](jwks-opzetten.md) endpoint. This proves that a JWT is signed by a party in possession of the private-key. Signing the JWT is most easily performed using a JWT library for the relevant programming language.

{% hint style="info" %}
The [JWT debugger](https://jwt.io/?debug) is a great place to see, at runtime, what the contents of the JWT are and what the token looks like. Note that we work with key pairs and not shared secrets. So make sure to select algorithms that work with key pairs like `RS` and `ES`.
{% endhint %}

{% hint style="info" %}
RSA is more complicated to implement than, say, HMAC algorithms. However, it is a lot more secure. For example, there is no shared secret. Also, using JWKS, it is possible to quickly rotate keys.
{% endhint %}

## Topics

[TOP-KT-005c - Applicatie toegang: SMART on FHIR backend services](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125356/TOP-KT-005c+-+Applicatie+toegang+SMART+on+FHIR+backend+services)
