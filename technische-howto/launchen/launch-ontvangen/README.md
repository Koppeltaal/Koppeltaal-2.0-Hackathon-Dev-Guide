# Receiving a HTI Launch

### Requirements

* The application [created](../../resources-managen/crud-operaties/resource-aanmaken.md) an [`ActivityDefinition`](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition).

#### When the application does not use [Token Introspection](token-introspection.md):&#x20;

* The application must be able to map the `issuer` to its corresponding JWKS endpoint.

### Verify the JWT

Incoming HTI launches have a `token` parameter. This value represents the signed JWT (see [Compose a launch](../launch-samenstellen/))

#### Token Introspection

The easiest and safest way to verify the JWT is to use [Token Introspection](token-introspection.md). This way, the application itself does not have to verify all the security checks on the incoming JWT. The token can simply be forwarded to the authentication server and it will perform all the required checks.&#x20;

#### Verify the JWT yourself

Using the issuer and the JWKS endpoint, the application can validate whether the JWT is actually signed by the private key of the asymmetric key pair. The JWK can be found using the kid (key id) field from the JWT Header section. For example:

![jwt.io debugger](../../../.gitbook/assets/screenshot-2021-09-22-at-20.22.09.png)

This can be mapped to JWK objects from the JWKS endpoint:

```javascript
{
  "keys": [
    {
      "kty": "RSA",
      "kid": "oHV39B4Ow8hcw3SbkF4k0w",
      "use": "sig",
      "n": "ngtOwJ9jxRyRL1HSlVVF-TI6iv2Pf3kWm2fereynTNz06tZaOo8hm6FA0fucLHH1OxsFFa6rDNyVZcivbR3uEO4CKXrzDw5HUDxLh9qaR-PMolsz4s2mrz6xP9kGkVvQvuzJPQTQ4fat6aI7p0cyA5xA8vqUBqeYU323zcf8uUXb-qSJ3FpidDnTKLksD7B-yjU1HjDdDysRp8pNL6lVs5KW2L8XMCpbrvhyIQ_c5pwOrF_QzfsK9mW78wCKLJZ_D7eVw2yututBzm9z0pq5PColQP2nB7lq98DNJvDbmvMIfDNglRKTS-Qqky_S8a6H5gfgFTv77iaaFFjjld4cJw",
      "e": "AQAB"
    }
  ]
}
```

The JWK represents the public key that can be used to validate the signature.

### Other important checks&#x20;

In addition to verifying the signature, the JWT payload contains fields that are important to validate.

| JWT claim (attribute)   | Details                                                                                                                                      | Implementation                                    |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `exp` (expiration time) | After this time, the JWT is no longer valid                                                                                                  | Is often done automatically by JWT libraries      |
| `iat`  (issued at)      | Issue date, it must not be in the future                                                                                                     | Is often done automatically by JWT libraries      |
| `jti` (JWT ID)          | Unique identifier for this JWT. The `jti` values used must be tracked. If a `jti` value has already been consumed, the JWT must be rejected. | Should most likely be implemented in custom logic |
