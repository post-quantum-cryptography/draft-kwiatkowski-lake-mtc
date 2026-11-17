Merkle Tree Certificates (MTC) {{I-D.ietf-plants-merkle-tree-certs}} change the way
a Certification Authority (CA) vouches for the certificates it issues. Instead of
signing every certificate individually, as in a conventional PKI, an MTC CA appends
each certificate it issues to an issuance log, which is a Merkle tree in the style
of Certificate Transparency {{RFC9162}}. At regular intervals, the CA designates the
current tree size as a landmark and publishes the hashes of the subtrees that cover
the log up to that size; a set of cosigners checks that the log has only grown and
signs these subtree hashes. A certificate then carries an inclusion proof in the
place of the CA signature: the proof lists the sibling hashes along the path from
the certificate's leaf up to a subtree root covered by a landmark, and the relying
party recomputes this path and compares the result against a landmark it already
trusts.

This construction is attractive for devices described in the taxonomy of
{{RFC7228}}, because it replaces the CA signature, which grows large with
post-quantum signature algorithms such as ML-DSA {{NIST-FIPS-204}}, with an
inclusion proof whose size grows only with the logarithm of the number of
certificates in the log. A landmark-relative certificate for a log of a million
entries carries about twenty 32-byte hashes, and verifying it costs only a few
SHA-256 computations, which even a class 1 device performs quickly.

The Lightweight Authenticated Key Exchange (LAKE) protocol {{RFC9528}} is a
lightweight authenticated key exchange protocol designed for constrained
devices and networks. This document specifies how to carry MTC credentials
in LAKE: how a certificate is encoded, and a new External Authorization Data
(EAD) item that lets the two peers negotiate, for each message, whether the
compact landmark-relative form of a certificate or its self-contained
standalone form is sent. This negotiation lets a device keep presenting the
smallest certificate form its peer can verify, without assuming that the
peer already holds the same landmarks, while every verifier remains able to
fall back to the standalone form that any relying party can check.
