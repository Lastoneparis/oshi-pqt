# Roadmap

## Phase 0 — public design

- [x] Publish goals, threat model, and conservative profile.
- [ ] Collect cryptography, browser, CA, and operator feedback.
- [ ] Track IETF TLS and IANA assignments rather than forking them.

## Phase 1 — reproducible prototype

- [ ] Build a test harness around an audited TLS library with hybrid-group support.
- [ ] Publish packet captures, configuration, performance results, and negative tests.
- [ ] Add CI for conformance and dependency scanning.

## Phase 2 — ecosystem trial

- [ ] Run opt-in, non-sensitive endpoints in Observe mode.
- [ ] Measure handshake size, failure rates, CPU cost, and middlebox behavior.
- [ ] Commission an independent security review.

## Phase 3 — standards-aligned deployment

- [ ] Adopt finalized IETF/TLS and CA/Browser Forum guidance.
- [ ] Deploy hybrid key establishment broadly.
- [ ] Graduate to PQ authentication only after client and WebPKI support is real.
