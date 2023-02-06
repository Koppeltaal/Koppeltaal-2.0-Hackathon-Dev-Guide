# Koppeltaal server metadata opvragen

De Koppeltaal Server biedt een [`CapabilityStatement`](https://www.hl7.org/fhir/capabilitystatement.html) aan. Deze statement bevat handige metadata m.b.t. de Koppeltaal Server. Zo wordt er bijvoorbeeld aangegeven wat de URL van de authenticatie server is.

Binnen de applicatie hoeven dus geen hard-coded URLs geconfigureerd te worden die al bekend zijn bij de Koppeltaal Server. Zie bijvoorbeeld `CapabilityStatement.rest.security` in de [Koppeltaal `CapabilityStatement`](https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata). Deze sectie bevat OAuth URLs conform de [OAuth extensie](https://www.hl7.org/fhir/extension-oauth-uris.html).

Naast het aanbieden van URLs, beschrijft de `CapabilityStatement` nog veel meer. Bijvoorbeeld welke resources en functionaliteiten de Koppeltaal Server aanbiedt.&#x20;
