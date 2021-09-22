# SHOF Launch Ontvangen



### Requirements

1. De applicatie moet een [`ActivityDefinition`](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition) hebben [aangemaakt](../resources-managen/crud-operaties/resource-aanmaken.md).

### Flow

![bron: https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md](../../.gitbook/assets/image%20%281%29.png)

1. Bij binnenkomst van de launch wordt de conformance opgehaald bij de Koppeltaal Server. Hier kan de [authorize & token URL](smart-hti-on-fhir-launch-ontvangen.md#token-url-metadata) opgevraagd worden.
2. Er wordt een redirect gestuurd naar de authorize URL met de `launch` token. Dit geeft een `code` terug.
3. De token URL wordt uitgevoerd. Hierbij wordt  de`code` omgeruild voor:
   1. Een `id_token` \(bevat informatie  over de gebruiker\).
   2. Een no-op `access_token` \(niet te gebruiken op  de Koppeltaal Server omdat deze user-specific is\).
   3. Een `context` object, deze vult de auth server a.d.h.v. het JWT token die als `launch` param is meegegeven.

A.d.h.v. het context object kan bepaald worden wie met welke rol ingelogd is op het systeem.

### Token & Authorize URL - Metadata

De Token URL kan gevonden worden in de Koppeltaal Server `CapabilityStatement` [https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata](https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata):

![](../../.gitbook/assets/screenshot-2021-09-22-at-21.11.54.png)



