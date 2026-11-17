## Exposure of the Landmark State Item {#exposure-of-the-landmark-state-item}

The protection that LAKE affords to each message differs, and the exposure
of the landmark state item defined in {{landmark-negotiation-in-lake}}
differs with it. `EAD_1` travels in the clear, so an observer learns which
CAs the initiator trusts and how recent its cache is. `EAD_2` is encrypted and
covered by the responder's authentication, and `EAD_3` and `EAD_4` are fully
protected. An attacker who alters or removes the item in `EAD_1` cannot remain
unnoticed, because `message_1` enters the transcript hash that both peers
authenticate; the worst outcome of such interference is a failed exchange or a
fallback to the larger standalone form.

## Landmark Cache Rollback Protection {#landmark-cache-rollback-protection}

The anchor described in {{landmark-synchronisation}} protects a device's
landmark cache against rollback.

Network adversary:
: It is the adversary LAKE is designed to resist. An ordinary non-volatile
  storage suffices, because the device accepts only packages whose landmark
  numbers continue from its newest landmark and, in end-to-end mode, whose
  consistency proof starts at its anchor.

An adversary with physical access or on-device code execution ability:
: The anchor needs to reside in storage that is authenticated and rollback-resistant,
  such as a monotonic counter in a secure element, a replay-protected memory block,
  or one-time-programmable fuses. Many class 1 devices offer none of these, and
  this document does not assume that they do.

A device without rollback-resistant storage keeps its anchor in ordinary
flash, protected by a message authentication code under a device key. If
the anchor is missing or fails this check, the device MUST clear its
landmark cache, accept only standalone certificates, and announce no
landmark state. It leaves this recovery mode with the first package that
passes end-to-end verification against the cosigner keys provisioned at
manufacture, or with an update from its designated server in delegated
mode, adopting the reference checkpoint of that package as its new anchor.
