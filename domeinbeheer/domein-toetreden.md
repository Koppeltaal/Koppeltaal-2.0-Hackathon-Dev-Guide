# Domein toetreden

Om een domein toe te treden kan een verzoek ingediend worden bij [Domeinbeheer](https://smart-backend-services.koppeltaal.headease.nl/register). Na het inloggen binnen Domeinbeheer worden momenteel alle schermen getoond \(zowel beheer als de eigenlijke  aanvraag\). Volg de volgende stappen om een domein toe te treden:

1. Navigeer naar [Domeinbeheer](https://smart-backend-services.koppeltaal.headease.nl/register).
2. Log via uw mobiel in met de [IRMA app](https://irma.app/).
3. Open de tab "AANVRAAG INDIENEN"
4. Voer een naam in en de JWKS endpoint van uw applicatie.
5. Submit de aanvraag.
6. Open de tab "OVERZICHT".
7. Ken een rol toe aan de toegevoegde applicatie.
8. Verander de status naar `Approved`.
9. Gebruik de `client_id` in uw applicatie om een [SMART Backend Service](../technische-howto/connectie-maken-met-koppeltaal/smart-backend-service.md) request uit te voeren.

{% hint style="info" %}
Koppeltaal vraagt alle applicaties om gebruik te maken van JWKS endpoints bij het opvragen van een `access_token`.   
  
Wanneer applicaties lokaal getest worden is het erg lastig om JWKS te gebruiken. In dat geval is het ook mogelijk om  simpelweg een public key in PEM formaat te registreren.
{% endhint %}

