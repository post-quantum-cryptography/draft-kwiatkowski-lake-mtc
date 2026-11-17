## Landmark Negotiation in LAKE {#landmark-negotiation-in-lake}

LAKE lets applications attach data to each of its messages through External
Authorization Data (EAD) fields {{RFC9528}}. An `EAD` field is a sequence of
items, each made of an integer label and an optional byte string value, and
the sign of the label indicates to the receiver whether the item is critical.
This document defines one new item, called landmark state. Its value is a
CBOR array that lists, for each CA that the sender trusts, the CA identifier
together with the oldest and newest landmark numbers it holds. Because a
verifier keeps a contiguous range of landmarks per CA, this range describes
its whole cache in a few tens of bytes.

The initiator places its landmark state in `EAD_1`. The responder compares it
with the landmark that covers its own certificate and sends the
landmark-relative form in `message_2` when the initiator holds that landmark,
and the standalone form otherwise. The responder places its own landmark
state in `EAD_2`, and the initiator makes the same choice for `message_3`. The
following diagram summarizes the exchange:

~~~ aasvg
Initiator (device)                                 Responder (server)
|                                                                   |
| message_1: G_X, C_I, EAD_1: landmark state of I                   |
+------------------------------------------------------------------>|
|                                         chooses the form of MTC_R |
| message_2: G_Y, Enc(C_R, ID_CRED_R: MTC_R, Sig_R,                 |
|             EAD_2: landmark state of R, opt. package)             |
|<------------------------------------------------------------------+
| verifies MTC_R and Sig_R, chooses the form of MTC_I               |
| message_3: Enc(ID_CRED_I: MTC_I, Sig_I, EAD_3)                    |
+------------------------------------------------------------------>|
|                                          verifies MTC_I and Sig_I |
| message_4: Enc(EAD_4: update package)                             |
|<------------------------------------------------------------------+
| verifies the package, updates its landmark cache                  |
~~~
{: title="LAKE exchange with MTC credentials"}

The landmark state item is non-critical: a peer that does not understand it
ignores it, and the sender falls back to the standalone form, which every
verifier can check. If a verifier nevertheless receives a form it cannot
verify, it MUST abort with a LAKE error, and the initiator restarts the
exchange with the standalone form. {{exposure-of-the-landmark-state-item}}
discusses the exposure of the landmark state item across these messages.
