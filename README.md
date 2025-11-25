<h1 align="center">@foxleys-rs/curve25519</h1>
<p align="center">
  <a href="https://nodejs.org/"><img src="https://img.shields.io/badge/Node.js-%3E%3D18-blue.svg" alt="Node.js ≥18" /></a>
  <a href="https://www.npmjs.com/package/@foxleys-rs/curve25519"><img src="https://badge.fury.io/js/%40foxleys-rs%2Fcurve25519.svg" alt="npm version" /></a>
  <a href="https://www.npmjs.com/package/@foxleys-rs/curve25519"><img src="https://img.shields.io/npm/dt/%40foxleys-rs%2Fcurve25519.svg" alt="npm downloads" /></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="MIT License" /></a>
</p>

<p align="center">
  <strong>High-performance Curve25519 implementation for Node.js powered by Rust (napi-rs)</strong><br>
  Includes X25519 key exchange, Ed25519 signing and verification with native speed.
</p>

---

## Table of Contents

- [Installation](#installation)
- [Supported Platforms](#supported-platforms)
- [Features](#features)
- [Usage](#usage)
- [API](#api)
- [Performance](#performance)
- [License](#license)

---

## Installation

```bash
npm install @foxleys-rs/curve25519
```

or with Yarn / pnpm / Bun:

```bash
yarn add @foxleys-rs/curve25519
pnpm add @foxleys-rs/curve25519
bun add @foxleys-rs/curve25519
```

This package ships prebuilt native binaries via napi-rs for all major platforms (no Rust toolchain required).

---

## Supported Platforms

| Operating System | Architecture | libc          | Prebuilt binaries |
|------------------|--------------|---------------|-------------------|
| Linux            | x86_64       | glibc         | Yes            |
| Linux            | x86_64       | musl          | Yes            |
| Linux            | arm64        | glibc         | Yes            |
| Linux            | arm64        | musl          | Yes            |
| macOS            | x86_64       | —             | Yes            |
| macOS            | arm64        | —             | Yes            |
| Windows          | x86_64       | —             | Yes            |
| Windows          | arm64        | —             | Yes            |

---

## Features

- X25519 key pair generation and public key derivation
- ECDH shared secret computation (X25519)
- Ed25519 signing and signature verification
- Extremely fast Rust core via napi-rs
- Zero-dependency native binaries for Node.js ≥18
- Automatic pure JavaScript fallback (curve25519-js) in environments without native support (e.g., some bundlers or testing)

---

## Usage

```js
/*
CJS
import curve from '@foxleys-rs/curve25519';
const {
  generateKeyPair,
  getPublicFromprivKey,
  calculateAgreement,
  calculateSignature,
  verifySignature,
} = curve;
*/
// ESM
import {
	generateKeyPair,
	getPublicFromPrivateKey,
	calculateAgreement,
	calculateSignature,
	verifySignature
} from "@foxleys-rs/curve25519";

// Generate a new key pair
const { pubKey, privKey } = generateKeyPair();

console.log("Public key :", pubKey);
console.log("Private key:", privKey);

// Derive public key from an existing private key (optional)
const pubKey2 = getPublicFromPrivateKey(privKey);

// Perform ECDH key exchange
const sharedSecret = calculateAgreement(pubKey, privKey);
console.log("Shared secret:", sharedSecret);

// Sign a message (Ed25519)
const message = Buffer.from("Hello, Curve25519!");
const signature = calculateSignature(privKey, message);

// Verify signature
const isValid = verifySignature(pubKey, message, signature);
console.log("Signature valid:", isValid);
```

All functions accept `Buffer` as input.

---

## API

| Function                        | Description                                      | Returns                         |
|---------------------------------|--------------------------------------------------|---------------------------------|
| `generateKeyPair()`             | Generates a new X25519 key pair                  | `{ privKey: Buffer, pubKey: Buffer }` |
| `getPublicFromPrivateKey(privKey)`   | Derives public key from private key              | `Buffer` (33 bytes)         |
| `calculateAgreement(pubKey, privKey)` | Computes X25519 shared secret                    | `Buffer` (32 bytes)         |
| `calculateSignature(privKey, msg)` | Signs message with Ed25519                       | `Buffer` (64 bytes)         |
| `verifySignature(pubKey, msg, sig)`| Verifies Ed25519 signature                       | `boolean`                       |

---

## Performance

Benchmarks performed on Node.js 25. Results show average latency and throughput:

| Task                            | Latency (avg)      | Throughput (avg ops/s) |
|---------------------------------|--------------------|------------------------|
| Rust generateKeyPair            | 75.1 μs ± 0.80%    | 13,610                 |
| Node generateKeyPair            | 237.0 μs ± 3.90%   | 5,225                  |
| Rust getPublicFromPrivateKey    | 209.1 μs ± 0.78%   | 4,876                  |
| Node getPublicFromPrivateKey    | 1,735.8 μs ± 2.84% | 606                    |
| Rust calculateAgreement         | 205.9 μs ± 0.43%   | 4,892                  |
| Node calculateAgreement         | 379.0 μs ± 2.44%   | 2,835                  |
| Rust calculateSignature         | 134.2 μs ± 0.27%   | 7,492                  |
| Node calculateSignature         | 37,241.7 μs ± 15.48% | 35                   |
| Rust verifySignature            | 229.6 μs ± 0.37%   | 4,389                  |
| Node verifySignature            | 30,703.2 μs ± 17.43% | 45                   |

The Rust implementation is **2–5× faster** than equivalent pure JavaScript implementations, especially for signing and verification.

---

## License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

Developed with ❤️ by Foxleys.