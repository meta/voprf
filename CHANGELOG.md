# Changelog

## 0.6.0-pre.2 (November 5, 2026)
Migrate to the RustCrypto 0.14 / `hybrid-array` ecosystem (thanks to
[nbari](https://github.com/nbari) for #159). This removes the dependency on
`crypto-bigint` 0.5 affected by
[RUSTSEC-2026-0387](https://rustsec.org/advisories/RUSTSEC-2026-0387.html). RFC 9497
test vectors and serialized bytes are unchanged.

* MSRV bumped to 1.88
* Updated elliptic-curve dependency to 0.14.3 (requiring the patched crypto-bigint
  0.7.8 or newer), and added the hash2curve 0.14 dependency (hash-to-curve and the
  OPRF curve parameters moved to this crate)
* Updated digest dependency to 0.11
* Updated curve25519-dalek dependency to 5
* Updated rand_core dependency to 0.10 and rand dev-dependency to 0.10
  (thanks to [asterized](https://github.com/asterized) for #157)
* Updated sha2 dependency to 0.11 and p256/p384/p521 dev-dependencies to 0.14
* Replaced generic-array with hybrid-array 0.4: public APIs now use
  `hybrid_array::Array` instead of `GenericArray`, and `Group::ElemLen` and
  `Group::ScalarLen` are `hybrid_array::ArraySize`
* RNG bounds changed from `TryRngCore + TryCryptoRng` to `TryCryptoRng`. The
  internal `CompatRng` adapter has been removed (resolves #150), so randomness
  failures return `Error::Rng` for the NIST curves too, instead of panicking
* Added `Group::SecurityLevel: Unsigned + Mul<U2>`, the target security level in
  bytes; the hash used by `Group::hash_to_curve()` and
  `Group::hash_to_scalar()` (and `CipherSuite::Hash`) must now output at least
  twice this many bytes, as required by RFC 9380
* The `CipherSuite` where clause changed: it no longer requires `IsLess<U256>`
  on the hash output size, and now requires
  `IsLessOrEqual<BlockSize, Output = True>` plus
  `IsGreaterOrEqual<Prod<<Self::Group as Group>::SecurityLevel, U2>, Output = True>`.
  Downstream code that repeats these bounds (e.g. opaque-ke) must be updated
* The blanket `CipherSuite` implementation is now for curves implementing
  `hash2curve::OprfParameters` (replacing `elliptic_curve::VoprfParameters`)
* Parts of this migration were adapted from the `sid-voprf` crate of the
  [structured-id/sid](https://github.com/structured-id/sid) project

## 0.6.0-pre.1 (April 6, 2026)
* MSRV bumped to 1.85
* Updated rand_core dependency to 0.9
* Updated rand dependency to 0.9
* Updated subtle dependency to 2.6
* Fixed docs issue

## 0.6.0-pre.0 (November 8, 2025)
* MSRV bumped to 1.83
* Updated Ristretto255 random scalar generation
* Updated generic-array to v1

## 0.5.0 (March 6, 2024)
* Just a version bump from v0.5.0-pre.7

## 0.5.0-pre.7 (January 11, 2024)
* Updated to be in sync with RFC 9497

## 0.5.0-pre.6 (July 24, 2023)
* Updated curve25519-dalek dependency to 4

## 0.5.0-pre.5 (June 27, 2023)
* Updated curve25519-dalek dependency to 4.0.0-rc.3

## 0.5.0-pre.4 (May 20, 2023)
* Updated curve25519-dalek dependency to 4.0.0-rc.2

## 0.5.0-pre.3 (March 4, 2023)
* Updated to be in sync with draft-irtf-cfrg-voprf-19
* Increased MSRV to 1.65
* Updated p256 dependency to v0.13
* Added p384 tests

## 0.5.0-pre.2 (February 3, 2023)
* Increased MSRV to 1.60
* Updated p256 dependency to v0.12
* Updated curve25519-dalek dependency to 4.0.0-rc.1

## 0.5.0-pre.1 (December 19, 2022)
* Updated curve25519-dalek dependency to 4.0.0-pre.5

## 0.4.0 (September 15, 2022)
* Updated to be in sync with draft-irtf-cfrg-voprf-11, with
  the addition of the POPRF mode
* Adds the evaluate() function to the servers to calculate the output of the OPRF
  directly
* Renames the former evaluate() function to blind_evaluate to match the spec
* Fixes the order of parameters for PoprfClient::blind to align it with the
  other clients
* Exposes the derive_key function under the "danger" feature
* Added support for running the API without performing allocations
* Revamped the way the Group trait was used, so as to be more easily
  extendable to other groups
* Added common traits for each public-facing struct, including serde
  support

## 0.3.0 (October 25, 2021)

* Updated to be in sync with draft-irtf-cfrg-voprf-08

## 0.2.0 (October 18, 2021)

* Removed the CipherSuite interface
* Added the "danger" feature for exposing internal functions
* General improvements to the group interface

## 0.1.0 (September 29, 2021)

* Initial release
