# Threat model

## Protected

OSHI aims to protect recorded TLS sessions against a future cryptanalyst with a cryptographically relevant quantum computer, assuming the selected hybrid TLS group and its implementation remain sound. It also retains TLS 1.3 protection against active network attackers, subject to normal certificate validation.

## Assumptions

- At least one component in the standardized hybrid key-establishment construction remains secure.
- ML-KEM and ML-DSA implementations are constant-time where required and use sound randomness.
- The endpoint, private keys, CA ecosystem, DNS routing, and client device are not already compromised.
- The TLS library implements the standardized group correctly.

## Not solved

- Compromised endpoints, malicious CAs, phishing, malware, traffic analysis, and availability attacks.
- Secrets already exposed at either endpoint.
- Long-term identity assurance until PQ certificate validation is standardized and deployed.
- Risks from experimental algorithms, implementation bugs, side channels, or protocol ossification.

## Review checklist

Before any production rollout: obtain an external cryptographic review, test against the relevant TLS interoperability suite, fuzz all key parsing paths, measure handshake amplification and CPU/memory limits, and document a rollback plan.
