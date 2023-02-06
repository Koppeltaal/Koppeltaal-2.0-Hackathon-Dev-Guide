# Requirements

An `access_token` is required to access the Koppeltaal server. For this, the following steps must be performed:&#x20;

1. The application possesses a [key pair](key-pair-maken.md) and can sign [JWTs](jwt-ondertekenen.md).&#x20;
2. The application has a [JWKS endpoint](jwks-opzetten.md).&#x20;
3. The application has [joined the domain](../../../domeinbeheer/domein-toetreden.md) with the JWKS endpoint set up and possesses a `client_id`.&#x20;
4. The application has a [role](../../../domeinbeheer/rollen-beheren/) and the status "Active".
