This document considers three parties: a constrained device, an online server
or gateway with which the device communicates, and an MTC CA that records every
certificate it issues in an issuance log checked by a set of independent
cosigners. The device and the server authenticate each other with certificates
carried in LAKE messages during key establishment. The server relays the
device's certificate requests to the CA, stays current with the CA's landmarks,
and verifies the device's certificates with hashes rather than signatures.

Solution uses CBOR signed and encoded certificate ({{?I-D.ietf-cose-cbor-encoded-cert}}). The CA logs the CBOR TBS structure itself,
which requires a new issuance log entry type and requires that the CA support
it.

{::include-nested sections/cbor-encoding.md}

{::include-nested sections/landmark-negotiation.md}

{::include-nested sections/landmark-synchronisation.md}
