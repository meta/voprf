# voprf ![Build Status](https://github.com/novifinancial/voprf/workflows/Rust%20CI/badge.svg)
An implementation of a (verifiable) oblivious pseudorandom function (VOPRF)

A VOPRF is a verifiable oblivious pseudorandom function, a protocol between a client and a server. The regular (non-verifiable) OPRF is also supported in this implementation.

This implementation is based on [RFC 9497](https://www.rfc-editor.org/rfc/rfc9497).

Documentation
-------------

The API can be found [here](https://docs.rs/voprf/) along with an example for usage.

Installation
------------

Add the following line to the dependencies of your `Cargo.toml`:

```
voprf = "0.6.0-pre.1"
```

### Minimum Supported Rust Version

Rust **1.87** or higher.

### RustCrypto API migration

The minimum `elliptic-curve` version is 0.14.3, requiring `crypto-bigint` 0.7.8
or newer to address [RUSTSEC-2026-0387](https://rustsec.org/advisories/RUSTSEC-2026-0387.html).
This implementation uses `hash2curve` 0.14,
`digest` 0.11 and `rand_core` 0.10. Primitive and serialized buffers use
`hybrid_array::Array`; their byte encodings and RFC 9497 suite identifiers remain
unchanged. Custom `Group` implementations specify their security level in bytes
through `Group::SecurityLevel`, matching the hash-to-curve parameters.

Random generators implement `TryRng` and `TryCryptoRng`. Randomness failures return
`Error::Rng` for both Ristretto255 and the NIST curves, without producing a scalar.
Cargo's MSRV-aware resolver selects dependency releases compatible with the
declared Rust version.

Contributors
------------

The author of this code is Kevin Lewi ([@kevinlewi](https://github.com/kevinlewi)).
To learn more about contributing to this project, [see this document](./CONTRIBUTING.md).

License
-------

This project is dual-licensed under either the [MIT license](./LICENSE-MIT)
or the [Apache License, Version 2.0](./LICENSE-APACHE).
You may select, at your option, one of the above-listed licenses.
