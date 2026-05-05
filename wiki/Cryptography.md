# Cryptography

## Algorithm Stack

| Purpose | Algorithm |
|---------|-----------|
| Key exchange | ECDH P-256 |
| Key derivation | HKDF SHA-256 |
| Encryption | AES-256-GCM |
| Forward secrecy | Double Ratchet |
| Key storage | IndexedDB + extractable: false |
| Peer verification | Safety Numbers (SHA-256 fingerprint) |

## Key Exchange

Session establishment uses a pre-key scheme:

1. Each user generates and publishes pre-keys to the server on first launch
2. When Alice wants to message Bob (even if offline), she fetches Bob's pre-keys
3. Both sides compute 4 ECDH operations and derive a master secret via HKDF
4. Session is established without requiring Bob to be online

## Key Derivation (HKDF)

ECDH produces a raw shared secret. HKDF transforms it into usable keys:

```
ECDH shared secret
  ↓ HKDF SHA-256
  ├── AES-256 encryption key
  ├── authentication key
  └── next ratchet key
```

## Double Ratchet

Every message is encrypted with a unique key:

```
Message 1: AES-GCM(key1) → key2 = HKDF(key1) → delete key1
Message 2: AES-GCM(key2) → key3 = HKDF(key2) → delete key2
...
[new DH exchange] → new master secret → reset chain
```

- **Forward secrecy**: past messages cannot be decrypted even if current key is compromised
- **Break-in recovery**: after compromise, next DH exchange restores security

## Key Storage

Keys are stored using `extractable: false` in IndexedDB:

```js
const key = await crypto.subtle.generateKey(
    { name: "ECDH", namedCurve: "P-256" },
    false, // cannot be exported from browser
    ["deriveKey"]
)
```

The browser holds the key internally — JavaScript cannot access the raw bytes.

## Safety Numbers

Users can verify they are talking to the right person by comparing fingerprints out-of-band (voice, video, or in person):

```js
const fingerprint = SHA-256(sort(myPubKey + theirPubKey))
// displayed as: "1234 5678 9012 3456 ..."
```

If fingerprints match — no MITM, server did not substitute keys.

## Code Auditability

All cryptographic code is in `trustgram-crypto` — a single isolated file with zero dependencies, using only the browser's built-in Web Crypto API. Anyone can inspect it via browser DevTools (F12 → Sources).

Integrity is enforced via SRI:
```html
<script src="https://trustgram-crypto.pages.dev/crypto.js" integrity="sha384-..."></script>
```
