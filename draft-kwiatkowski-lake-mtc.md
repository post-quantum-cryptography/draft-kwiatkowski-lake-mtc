---
title: Merkle Tree Certificates for LAKE
abbrev: MTC for LAKE
category: std

docname: draft-kwiatkowski-lake-mtc-latest
submissiontype: IETF
number:
date:
stand_alone: yes
consensus: true
v: 3
ipr: trust200902
area: "Security"
workgroup: LAKE Working Group
keyword:
 - PKI
 - MTC
 - post-quantum
 - IoT

venue:
  group: "Lightweight Authenticated Key Exchange"
  type: "Working Group"
  mail: "lake@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/lake/"
  github: "post-quantum-cryptography/draft-kwiatkowski-lake-mtc"
  latest: ""

author:
  - ins: K. Kwiatkowski
    name: Krzysztof Kwiatkowski
    org: PQShield
    email: kris@amongbytes.com
    country: United Kingdom
  - ins: S. Raza
    name: Shahid Raza
    org: University of Glasgow
    email: shahid.raza@glasgow.ac.uk
    country: United Kingdom

normative:
  RFC9528:
informative:
  RFC9162:
  RFC7228:
  I-D.ietf-plants-merkle-tree-certs:
  NIST-FIPS-204: DOI.10.6028/NIST.FIPS.204
  NIST-FIPS-205: DOI.10.6028/NIST.FIPS.205

--- abstract

Merkle Tree Certificates (MTC) replace the signature of a Certification
Authority (CA) with smaller inclusion proof against a periodically
published landmark. The large sizes of post-quantum (PQC) keys and signatures
make conventional certificate chains costly to validate on constrained devices,
which have limited memory, compute, and message-size budgets. MTC's lightweight
properties keep certificate verification practical for such devices even
when PQC signature algorithms are used.

This document specifies how to carry MTC credentials in the Lightweight
Authenticated Key Exchange (LAKE) protocol {{RFC9528}}: CBOR encodings of MTC
certificates and landmarks, and a new External Authorization Data (EAD) item
that lets the two peers negotiate, per message, between the compact
landmark-relative form
of a certificate and its self-contained standalone form.

--- middle

# Introduction

{::include sections/introduction.md}

# Motivation

{::include sections/motivation.md}

# Terminology

{::include sections/terminology.md}

# Design

{::include-nested sections/design.md}

# Security Considerations

{::include sections/security-considerations.md}

# IANA Considerations

{::include sections/iana-considerations.md}
