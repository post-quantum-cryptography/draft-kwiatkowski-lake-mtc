None of the three post-quantum signature algorithms standardized by the US
National Institute of Standards and Technology (NIST) keeps certificate
chains within the message sizes and link budgets that LAKE and the networks
beneath it are designed for without a trade-off. FN-DSA keeps the smallest
signatures of the three, but even its smallest variant remains far larger
than the pre-quantum signatures it replaces, and reaching that size
requires an implementation that depends on carefully constructed Gaussian
sampling, and a flawed implementation can leak the signing key through
side channels.

SLH-DSA sits at the opposite end: its security rests only on the properties
of its underlying hash function, which makes it the most conservative
choice, both from security and implementation perspective, but its signatures
run into the tens of kilobytes and its signing is comparatively slow. A
conventional PKI must pick one of these trade-offs and pay its cost on every
certificate.

MTC decouples this choice from the cost paid by every certificate. Because
the CA and cosigners sign a landmark only once per interval rather
than once per certificate, this signature cost is amortised across every
certificate issued since the previous landmark: the CA and each cosigner
can pick whichever algorithm suits
their own trade-off between signature size, signing speed, and
implementation risk, including a conservative but large scheme such as
SLH-DSA {{NIST-FIPS-205}}, without passing that cost on to every certificate
a device presents, which still carries only a small inclusion proof
regardless of which algorithm the CA and cosigners chose.

Because every certificate appears in a log that a set of independent cosigners
checks, the operator of a device fleet gains a means to detect certificates
that were issued without authorisation, a property that a
conventional PKI does not provide without additional infrastructure such as
Certificate Transparency. MTC addresses only the cost of verifying a peer's
certificate. A device's own authentication signature in LAKE still depends
on whichever algorithm it holds a key for, with the memory, performance, and
side-channel costs that implementation entails
{{?I-D.ietf-pquip-pqc-hsm-constrained}}. Realising these benefits for
devices that authenticate with LAKE requires certificate encodings and a
negotiation mechanism that fit within LAKE's credential identifiers and EAD
fields.
