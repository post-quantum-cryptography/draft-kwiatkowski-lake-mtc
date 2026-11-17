{::boilerplate bcp14-tagged}

This document uses the terms defined in {{I-D.ietf-plants-merkle-tree-certs}},
including *issuance log*, *landmark*, *cosigner*, *inclusion proof*, *standalone
certificate*, and *landmark-relative certificate*. It also uses the device
classes defined in {{RFC7228}}. In addition, this document uses the following
terms:

Constrained device:
: A device as characterized in {{RFC7228}}, participating in the LAKE exchange
  as the party that authenticates with an MTC credential and that keeps a
  limited cache of landmarks.

Server:
: An online peer that a constrained device communicates with, and which relays
  the device's certificate requests and landmark updates to the MTC CA and its
  cosigners.

Landmark state:
: The range of landmark numbers, for a given CA, that a party holds in its
  cache, expressed as the oldest and newest landmark number it has for that
  CA.

Update package:
: A COSE_Sign1 structure, signed by the CA and countersigned by its cosigners,
  that communicates one or more new landmarks together with a consistency
  proof showing that the log has only grown.

Update server:
: The server that a device designates, in delegated mode, to perform the MTC
  checks on its behalf and to supply it with validated landmarks.

Delegated mode:
: A landmark validation mode in which a device trusts its designated update
  server to perform the MTC checks on new landmarks, rather than performing
  them itself.

End-to-end mode:
: A landmark validation mode in which a device performs the MTC checks on new
  landmarks itself, and can accept updates relayed by any peer.

Anchor:
: The state a device stores to protect its landmark cache against rollback,
  consisting of the number, tree size, and root hash of the newest landmark it
  has validated.
