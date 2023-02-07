# Request Koppeltaal server metadata

Koppeltaal metadata resides at two places

### CapabilityStatement

The Koppeltaal Server provides a [`CapabilityStatement`](https://www.hl7.org/fhir/r4/capabilitystatement.html). This statement contains useful metadata related to the Koppeltaal Server. For example, it indicates the supported Resource types and supported MIME types.

### SMART on FHIR Conformance

Koppeltaal also makes use of the [SMART on FHIR conformance](https://build.fhir.org/ig/HL7/smart-app-launch/conformance.html#conformance). This specifically contains metadata used whilst executing the [SMART on FHIR specifications](https://smarthealthit.org/). This conformance can be found `<BASE_FHIR_URL>/.well-known/smart-configuration`. This way, there is no need to configure hard-coded URLs. This configuration can be used to, for example, retrieve the Auth Server URL for the specific Koppeltaal server at runtime.
