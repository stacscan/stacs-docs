---
layout: default
title: Credential Types
parent: Support
permalink: /support/credential-types
---

## Credential Types
{: .fs-7 }

This section of the documentation details which tools STACS can integrate with, and what
credential formats can be detected.

### Cloud
{: .fs-6 }

* AWS credentials (access key id / secret access key).
* GCP service account JSON.
* Generic JSON Web Tokens (JWTs).
* Strip API tokens (secret keys).

### Hashes (Crypt)
{: .fs-6 }

* bcrypt.
* MD5.
* SHA1.
* SHA256.
* SHA512.
* NTHash.
 
### *Nix
{: .fs-6 }

* `/etc/passwd` - Missing password (blank).
* `/etc/passwd` - Static password.
* `/etc/shadow` - Missing password (blank).
* `/etc/shadow` - Static password.

### PKI
{: .fs-6 }

* PEM format DSA private key (PKCS#1)
* PEM format RSA private key (PKCS#1)
* PEM format private key (PKCS#8)

### SaaS
{: .fs-6 }

* Github Tokens.

