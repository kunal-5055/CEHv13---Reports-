# Module 20 — Cryptography 🔐🧩

> **Goal:** Build a practical, applied understanding of modern cryptography — primitives, protocols, common implementation pitfalls, and how cryptography is used (and misused) in real systems.

---


## Overview

Cryptography underpins confidentiality, integrity, authentication, and non-repudiation across modern systems. This module focuses on understanding algorithms (symmetric, asymmetric, hashing), practical protocols (TLS, SSH, JWT), secure usage patterns, and common mistakes that lead to real-world vulnerabilities.

---

## Learning objectives

By the end of this module you will be able to:

* Explain core cryptographic primitives and when to use them.
* Identify weak or incorrect cryptographic usage in code and configurations.
* Analyze TLS/SSH/JWT deployments and find common misconfigurations.
* Build simple cryptographic tools and write secure code using standard libraries.
* Understand attacks: padding oracle, timing/side-channel, RNG weaknesses, key management failures.

---

## Prerequisites

* Comfortable with programming (Python, or another high-level language).
* Basic number theory and probability helps but not required — key concepts will be introduced.
* Familiarity with networking and web technologies for protocol labs.

---

## Core topics

* **Symmetric crypto:** AES (modes: CBC, GCM), block vs stream ciphers, authenticated encryption.
* **Asymmetric crypto:** RSA, ECC (ECDSA, ECDH), key sizes, signature schemes.
* **Hashes & MACs:** SHA-family, HMAC, collision vs preimage resistance.
* **Key derivation & storage:** PBKDF2, HKDF, bcrypt, Argon2, key management principles.
* **Randomness:** CSPRNGs, entropy sources, failures.
* **Protocols & formats:** TLS (handshake, certs, ciphersuites), SSH, JWT, S/MIME, PGP.
* **PKI & certificates:** X.509, CA hierarchy, OCSP, certificate pinning.
* **Implementation pitfalls:** hardcoded keys, improper IV use, nonce reuse, incorrect padding, DIY crypto.

---

## Tools & libraries

* **Crypto libraries:** OpenSSL, libsodium, PyCryptodome, cryptography (Python).
* **Analysis & testing:** `openssl` CLI, `sslyze`, `testssl.sh`, `nmap --script ssl-*`, `jwt-tool`.
* **Fuzzing & exploit tools:** `tlsfuzzer`, `scapy`, `PaddingOracleTool`.
* **Useful languages:** Python (for quick PoCs), Go/Rust (for secure implementations).

---

## Course outline & lessons

1. **Fundamentals** — goals of crypto, threat model, basic math intuition.
2. **Symmetric crypto & AEAD** — modes, IV/nonce management, authenticated encryption.
3. **Asymmetric crypto & signatures** — RSA padding (PKCS#1v1.5 vs OAEP), ECDSA pitfalls.
4. **Hashes & MACs** — use cases, length-extension attacks, HMAC correctness.
5. **Randomness & key derivation** — CSPRNGs, PBKDFs, salts.
6. **TLS deep-dive** — handshake, ciphersuites, common misconfigurations, certificate validation.
7. **JWT & token security** — alg none, key confusion, token storage.
8. **Attacks & side-channels** — padding oracle, timing attacks, nonce reuse, RNG failures.
9. **Secure coding & libraries** — using high-level primitives safely, avoiding DIY crypto.
10. **Key management & PKI** — secure storage, rotation, secure provisioning.

---

## Hands-on labs / exercises (suggested)

Each lab should include objective, steps, PoC code, outputs/screenshots, and remediation notes.

### Lab A — Symmetric primitives & misuse

* Encrypt/decrypt files using AES-CBC vs AES-GCM; demonstrate forgery on CBC without HMAC.
* Show nonce reuse consequences with AES-GCM via small PoC script.

### Lab B — Padding Oracle Attack

* Deploy a purposely vulnerable service (CBC + oracle) and exploit it to recover plaintext.
* Provide safe, local-only PoC and explain fixes (use AEAD or constant-time decryption + MAC).

### Lab C — TLS configuration audit

* Use `testssl.sh` / `sslyze` to scan a server; interpret results (weak ciphers, missing SNI, obsolete protocols).
* Fix configuration examples for OpenSSL/nginx.

### Lab D — JWT & token attacks

* Create tokens with different `alg` headers (including `none`), demonstrate common server mishandling and remediation.

### Lab E — Key management & KMS

* Demonstrate storing secrets in HashiCorp Vault or cloud KMS, vs unsafe env var usage.

### Lab F — Side-channel basics (timing)

* Implement and measure a naive comparison vs constant-time compare; show exploitability with local timing measurements.

---

## Cheat sheet / quick commands & code snippets

* Generate RSA key and self-signed cert:
  `openssl genpkey -algorithm RSA -out key.pem -pkeyopt rsa_keygen_bits:2048`
  `openssl req -new -x509 -key key.pem -out cert.pem -days 365 -subj "/CN=example"`

* Test TLS server:
  `testssl.sh --fast example.com`
  `openssl s_client -connect example.com:443 -servername example.com`

* Simple AES-GCM encrypt/decrypt (Python, `cryptography` library):

```python
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
key = AESGCM.generate_key(bit_length=128)
aesgcm = AESGCM(key)
nonce = os.urandom(12)
cipher = aesgcm.encrypt(nonce, b"secret msg", None)
```

* Constant-time compare (Python): `hmac.compare_digest(a, b)`

---

## Deliverables & assessment

For each lab submit:

* Short report (max 2 pages) with objective, steps, outputs, and remediation.
* PoC scripts clearly labeled non-destructive.
* Short reflection on secure design choices and mitigation strategies.

Grading: correctness of exploitation, clarity, secure remediation proposals, and safe lab conduct.

---

## Safety, ethics & legal ⚖️

* Only test on systems you own or have explicit permission to test.
* Use local or isolated lab environments for vulnerable services.
* Avoid publishing private keys or sensitive data in reports; redact where necessary.

---


