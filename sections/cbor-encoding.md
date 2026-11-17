## CBOR Encoding of Certificates and Landmarks

The inclusion proof is a CBOR array {{?RFC8949}} containing the index range of
the subtree, the inclusion path as a sequence of 32-byte hashes (when SHA-256
is used), and, for the standalone form, the cosignatures over the subtree
encoded as COSE_Sign1 signatures {{?RFC9052}}. The landmark-relative form omits
the cosignatures. Since the certificate type and the algorithm code point
already identify an MTC credential, LAKE carries it with the existing
credential identifiers, `c5c` for C509 and `x5chain` for X.509 {{RFC9528}}, and
no new `ID_CRED` parameter is needed.

A landmark is encoded as a CBOR array containing the CA identifier, the
landmark number, the tree size, and the subtree hashes that cover the tree at
that size. The CA identifier reuses the trust anchor identifier of the MTC CA
{{?I-D.ietf-tls-trust-anchor-ids}}, encoded with the CBOR tag for object
identifiers {{?RFC9090}}. Landmark numbers are consecutive for a given CA
identifier, so the pair formed by the CA identifier and the landmark number
designates a landmark compactly.

Landmarks reach devices in update packages. An update package is a `COSE_Sign1`
object in which the CA signs the previous and new tree sizes, the new
landmarks, and a consistency proof showing that the new tree size extends the
old one, that is each cosigner adds a COSE countersignature {{?RFC9338}}. An
update package is therefore self-authenticating, and any party may relay it.
