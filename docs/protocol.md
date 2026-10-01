# OSHI Transport Profile 0.1

## 1. Scope

OSHI is a constrained TLS 1.3 profile for HTTP/3 endpoints. It defines migration requirements; it does not define new TLS messages, a new certificate format, or new cryptographic primitives.

Keywords such as **MUST**, **SHOULD**, and **MAY** are to be interpreted as described in RFC 2119 and RFC 8174.

## 2. Cryptographic profile

| Function | OSHI baseline | Why |
| --- | --- | --- |
| Transport | QUIC v1 + TLS 1.3 | Mature web transport and record protection |
| AEAD | AES-128-GCM or ChaCha20-Poly1305 | TLS 1.3 mandatory-to-implement choices |
| Hash / KDF | SHA-256 / HKDF-SHA-256 | TLS 1.3 schedule |
| Classical KEX | X25519 | Widely implemented, compact |
| PQ KEM | ML-KEM-768 | NIST FIPS 203 security/performance balance |
| Classical signature | Ed25519 or ECDSA P-256 | WebPKI compatibility |
| PQ signature | ML-DSA-65 | NIST FIPS 204 mid-level parameter set |

Exact named groups and signature-scheme identifiers MUST use the IANA assignments adopted by the underlying TLS implementation. OSHI MUST NOT allocate private values for production traffic.

## 3. Handshake

1. The client sends a normal TLS 1.3 ClientHello over QUIC and offers a standardized hybrid group combining X25519 and ML-KEM-768.
2. The server selects that group when supported and sends the corresponding TLS 1.3 ServerHello key share.
3. The TLS implementation derives the handshake secret using its standardized hybrid-group construction. Implementations MUST use the construction specified for the selected group; OSHI does not concatenate secrets itself.
4. The server proves identity with a certificate chain accepted by the client. During transition, OSHI-Observe permits a classical WebPKI signature. OSHI-Required additionally requires an authenticated ML-DSA proof delivered through a standardized TLS/X.509 mechanism.
5. Finished verification and application traffic use unmodified TLS 1.3 behavior.

## 4. Modes

| Mode | Hybrid KEX | PQ authentication | Intended use |
| --- | --- | --- | --- |
| Observe | preferred | optional | telemetry and interoperability |
| Negotiate | required when mutually offered | optional | broad migration |
| Required | required | required | high-value, reviewed deployments |

An endpoint MUST make its mode visible in configuration and logs. A client claiming OSHI-Required MUST fail closed if the selected connection does not meet its policy.

## 5. Downgrade resistance

- The selected group and all TLS extensions are transcript-bound by TLS 1.3.
- Clients MUST NOT retry a failed OSHI-Required connection with a weaker policy without explicit user or administrator policy.
- Servers SHOULD publish policy only through authenticated, cacheable, versioned mechanisms after an interoperable standard exists. This draft intentionally does not invent a header.
- 0-RTT data MUST remain disabled until the selected TLS stack’s replay and PQ properties are reviewed for the deployment.

## 6. Certificate transition

Today’s WebPKI clients may not validate PQ signatures or PQ certificate chains. OSHI therefore separates confidential key establishment from PQ authentication. Hybrid KEX can be deployed first in compatible TLS stacks; PQ authentication follows only with standardized browser, CA, and TLS support. Do not ship an ad-hoc parallel signature extension as a substitute for that work.

## 7. Interoperability requirements

- Implementations MUST expose the negotiated group, signature scheme, and mode to telemetry without logging secrets or full identifiers.
- Implementations MUST impose strict input-size limits before parsing KEM public keys and ciphertexts.
- Implementations SHOULD use constant-time, vetted provider code and upstream conformance tests.
- Implementations MUST retain ordinary TLS certificate validation, hostname verification, and revocation policy.
