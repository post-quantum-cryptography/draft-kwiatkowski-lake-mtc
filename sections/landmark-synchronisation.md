## Landmark Synchronisation

A device that verifies landmark-relative certificates MUST hold, for each CA
it trusts, the subtrees of every active landmark. A landmark remains active
until every certificate it covers has expired, so a CA that follows the
allocation procedure of {{I-D.ietf-plants-merkle-tree-certs}} keeps at most

~~~
W = ceil(T_cert / T_lm) + 1
~~~

landmarks active, where `T_cert` is the maximum certificate lifetime and `T_lm`
is the time between landmarks. Next to its landmarks, a device stores an
anchor made of the number of its newest landmark together with the tree size
and root hash of the checkpoint against which it last validated its cache;
every later update MUST extend this anchor.

How a device validates new landmarks depends on whom it trusts to deliver
them. This document defines two validation modes:

Delegated mode:
: The device designates one server, typically the enrollment server of the
  organisation that operates the device, as its update server. This server
  performs the MTC checks and sends the device only the number, tree size,
  and subtree hashes of each new landmark, protected by OSCORE {{?RFC8613}} or
  carried in `EAD_4` once the server has authenticated.

End-to-end mode:
: The device performs the MTC checks itself and can accept updates relayed
  by any peer. The update package then carries a reference checkpoint at
  least as large as the newest landmark, a consistency proof from the anchor
  to the reference checkpoint, and the subtree hashes of each new landmark
  with a subtree consistency proof towards the reference checkpoint.

An update proceeds in four steps: (1) the device, or its server, detects that
the cache lags behind, either because the landmark state received in `EAD_1`
or `EAD_2` is older than the peer's own, through a CoAP Observe {{?RFC7641}}
notification on the landmark resource, or on a schedule; (2) the device
obtains the update package, either embedded in `EAD_4` of `message_4` for small
increments, or by fetching it from the landmark resource of its designated
update server for larger catch-ups; (3) the device verifies the package
according to its validation mode; and (4) on success, the device commits the
result by writing the new landmarks into free storage, advancing its anchor
in a single atomic write, and then releasing the landmarks that have left
the active window. A failure at any step leaves the device able to
authenticate with standalone certificates, and no failure undoes state the
device has already committed.

The anchor also protects the cache against rollback
(see {{landmark-cache-rollback-protection}}) for the threat model this protects
against and for the recovery mode a device enters when that protection
fails.

Many constrained devices have no reliable clock, yet landmarks carry an
expiration time and certificates carry a validity period. Such a device can
still bound the age of a landmark by its number - that is, because the CA
allocates at most one landmark per interval, a landmark at least `W` numbers
older than the newest one the device holds has certainly expired and can
be discarded. This bound only advances when the device synchronises, and a
device that stays offline cannot tell how much time has passed and keeps
treating its last landmarks as active.

LAKE does not limit the size of EAD fields, so the practical limit on
message size comes from the transport and the link layer. The landmark state
item fits in `EAD_1` and adds little to `message_1`. For update packages,
landmark hashes contribute only 32 bytes per subtree, but each package
carries at least two signatures, from the CA and from one cosigner, and with
a post-quantum signature algorithm signatures size dominates the size of
the package. Implementations SHOULD deliver large updates through the fetch
mechanism described above and reserve `EAD_4` for small increments, and
MAY batch several landmarks into one update package to amortise the cost
of its signatures.
