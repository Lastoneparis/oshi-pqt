# OSHI Transport

<p align="center">
  <img src="assets/oshi-mark.png" alt="OSHI mark" width="160" />
</p>

<p align="center"><b>Open Secure Hybrid Internet</b><br />A practical post-quantum profile for the web.</p>

> **Status: proposal / research scaffold.** OSHI is not a replacement for HTTPS today, and it is not safe to deploy as a new cryptographic protocol without independent review and standardization. Its purpose is to make a conservative migration path easy to study and implement.

The web does not need to discard HTTPS to become quantum-resilient. OSHI defines a profile for **TLS 1.3 over HTTP/3** that uses hybrid key establishment and post-quantum authentication while retaining standard web semantics, PKI validation, and TLS’s battle-tested record layer.

## The idea

```
browser ── HTTP/3 ── TLS 1.3 + OSHI profile ── QUIC ── Internet
                         │
                         ├─ X25519 + ML-KEM-768      (hybrid secrecy)
                         └─ classical + ML-DSA auth  (hybrid identity)
```

An attacker who records traffic now should need to defeat *both* the classical and post-quantum components to recover the session secret later. The classical component keeps familiar security assumptions in play; the post-quantum component is designed for the “harvest now, decrypt later” threat.

## Design goals

- Preserve HTTP, URLs, origins, cookies, and web PKI.
- Build on TLS 1.3 and QUIC rather than inventing a record protocol.
- Use NIST-standardized ML-KEM and ML-DSA primitives.
- Make downgrade resistance, interoperability, and auditability first-class.
- Permit staged deployment: observe → negotiate → require.

## Non-goals

- A proprietary cipher suite or a claim to supersede IETF standards.
- Replacing certificates, browsers, DNS, or the WebPKI in version 0.
- “Quantum encryption” claims. OSHI is post-quantum cryptography, not QKD.

## Repository map

- [`docs/protocol.md`](docs/protocol.md) — normative-ish wire/profile proposal.
- [`docs/threat-model.md`](docs/threat-model.md) — assumptions and remaining risks.
- [`docs/roadmap.md`](docs/roadmap.md) — path from prototype to standardization.
- [`src/`](src) — reserved for a reference implementation.

## Quick start

There is deliberately no custom crypto implementation here. Prototype OSHI by configuring an audited TLS 1.3 stack that supports standardized hybrid groups, then verify a captured handshake against the requirements in [`docs/protocol.md`](docs/protocol.md).

## Contributing

Cryptographic changes require test vectors, a threat-model update, and review by people with relevant expertise. Please report vulnerabilities privately; see [SECURITY.md](SECURITY.md).

## License

Specification text: [CC BY 4.0](LICENSES/CC-BY-4.0.txt). Code: Apache-2.0.
