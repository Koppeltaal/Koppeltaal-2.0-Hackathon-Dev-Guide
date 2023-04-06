# Joining a domain

To join a domain, a request can be submitted to [Domain Management](https://domain-admin.koppeltaal.headease.nl/). After logging in to Domain Management, pages for all roles will be displayed (both management and requesting access). Follow these steps to join a domain:

1. Navigate to [Domain management](https://domain-admin.koppeltaal.headease.nl/).
2. Login via the [yivi app](https://www.yivi.app/) on your mobile device.
3. Open the tab "AANVRAAG INDIENEN"
4. Enter the name of you application and its corresponding [JWKS endpoint](../technische-howto/connectie-maken-met-koppeltaal/requirements/jwks-opzetten.md).
5. Submit the request.
6. Open the tab "OVERZICHT".
7. Assign a role to your newly created application.
8. Change the status to `Approved`.
9. Use the `client_id` in your application to execute a [SMART Backend Service](../technische-howto/connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md) request.

{% hint style="info" %}
Koppeltaal recommends all applications to use JWKS endpoints when requesting an `access_token`.&#x20;

When applications are tested locally, it is difficult to use JWKS. In that case it is also possible to simply register a public key in PEM format. [This](https://github.com/Koppeltaal/Koppeltaal-2.0-Generate-KeyPair/) project can be used to quickly generate a usable key pair for the Koppeltaal POC components.&#x20;

To test using JWKS locally, handy tools such as [ngrok](https://ngrok.com/) can be used.
{% endhint %}
