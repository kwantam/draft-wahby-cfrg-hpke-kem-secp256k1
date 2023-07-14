---
title: "secp256k1-based DHKEM for HPKE"
abbrev: "hpke-secp256k1-kem"
category: info

docname: draft-wahby-cfrg-hpke-kem-secp256k1-latest
submissiontype: IRTF
number:
date:
consensus: true
stand_alone: true
v: 1
area: "IRTF"
workgroup: "Crypto Forum"
keyword:
 - secp256k1
 - dhkem
 - hpke
venue:
  group: "Crypto Forum"
  type: "Research Group"
  mail: "cfrg@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/search/?email_list=cfrg"
  github: "kwantam/draft-wahby-cfrg-hpke-kem-secp256k1"
  latest: "https://github.com/kwantam/draft-wahby-cfrg-hpke-kem-secp256k1/"

author:
 -
    ins: R.S. Wabhy
    fullname: Riad Wahby
    organization: Carnegie Mellon University
    email: riad@cmu.edu

normative:
  RFC9180:
  SEC1v2:
    target: https://secg.org/sec1-v2.pdf
    title: "SEC 1: Elliptic Curve Cryptography"
    date: 2009
  SEC2v2:
    target: https://secg.org/sec2-v2.pdf
    title: "SEC 2: Recommended Elliptic Curve Domain Parameters"
    date: 2010

informative:

--- abstract

This memo defines DHKEM-secp256k1, a variant of HPKE DHKEM (RFC9180)
built on the secp256k1 elliptic curve.

--- middle

# Introduction

## Motivation

The secp256k1 elliptic curve is widely used in blockchain applications.
To date, several proposals have sought to allow users to use their keys
    for encryption.
To enable this application, this document specifies a DHKEM mode for use
    with the secp256k1 elliptic curve.
Several implementations appear to have sprung up ad-hoc; this document is
    written in hope of avoiding fragmentation in the ecosystem, particularly
    around HPKE KEM suite-id assignments.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Construction

The secp256k1 elliptic curve is specified in {{SEC2v2}}, Section 2.4.1.
DHKEM is specified in {{RFC9180}}, Section 4.

The secp256k1 DHKEM construction closely follows NIST-P256 DHKEM. See
    {{iana}} for the precise specification.

## Serializing and deserializing keys

Conversion functions in this section are defined in {{SEC1v2}}.

- The SerializePublicKey() function uses the uncompressed Elliptic-Curve-Point-to-Octet-String conversion.

- The DeserializePublicKey() function uses the uncompressed Octet-String-to-Elliptic-Curve-Point conversion.
  Deserialized public keys MUST be validated before they can be used in a
  manner analogous to the one for NIST-P256 in {{RFC9180}}, Section 7.1.4.

- The SerializePrivateKey() function uses the Field-Element-to-Octet-String conversion.
  If the private key is an integer outside the range [0, order-1], where 'order' is
  the order of the curve being used, the private key MUST be reduced to its
  representative in [0, order-1].

- The DeserializePrivateKey() function uses the Octet-String-to-Field-Element conversion.

## DeriveKeyPair

The DeriveKeyPair() function is as described in {{RFC9180}}, Section 7.1.3.
For this curve, the bitmask value 0xff should be used.
The order of the secp256k1 curve is given in {{SEC2v2}}.

# Security Considerations

Please consult the security considerations from {{RFC9180}}.

# IANA Considerations {#iana}

This document requests/registers a new entry to the "HPKE KEM Identifiers"
 registry.

 Value:
 : 0x0013 (please)

 KEM:
 : DHKEM(secp256k1, HKDF-SHA256)

 Nsecret:
 : 32

 Nenc:
 : 65

 Npk:
 : 65

 Nsk:
 : 32

 Auth:
 : yes

 Reference:
 : {{SEC2v2}}, {{RFC9180}}

--- back

# Acknowledgements

The author would like to thank
Christopher Wood for his input.

# Test Vectors

This section contains test vectors formatted similary to the ones
found in {{RFC9180}}.

--TODO--
