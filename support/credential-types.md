---
layout: default
title: Credential Types
parent: Support
permalink: /support/credential-types
---

## Credential Types
<br />
This section of the documentation details which tools STACS can integrate with, and what
credential formats can be detected.

### Cloud

* AWS credentials (access key id / secret access key).
* GCP service account JSON.

### Hashes (Crypt)

* bcrypt.
* MD5.
* SHA1.
* SHA256.
* SHA512.
* NTHash.
 
### *Nix

* `/etc/passwd` - Missing password (blank).
* `/etc/passwd` - Static password.
* `/etc/shadow` - Missing password (blank).
* `/etc/shadow` - Static password.

### PKI

* PEM format DSA private key (PKCS#1)
* PEM format RSA private key (PKCS#1)
* PEM format private key (PKCS#8)

### SaaS

* Github Tokens.

