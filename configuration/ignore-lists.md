---
layout: default
title: Ignore Lists
parent: Configuration
permalink: /configuration/ignore-lists
---

## Ignore Lists
{: .fs-7 }

Ignore lists provide a way for STACS users to 'suppress' known findings and false
positives. The most common use of ignore lists are ignoring certain directories or files
which may contain 'fake' credentials - such as those inside of tests.

It's important to note that suppressing a finding does not prevent STACS from recording
the finding.

Findings which match an ignore list entry are marked as suppressed, with the `reason`
copied from the ignore list into the suppression. This allows teams to still have
visibility over suppressed findings, and the reasons why they were suppressed.

### Includes
{: .fs-6 }

STACS allows ignore lists to "include" other ignore list files to allow composition of
ignore lists. This allows ignore lists to be maintained for different components within
a project, rather than having one large ignore list for the project.

"Includes" are especially useful in organisations where many of the same frameworks or
products are used. If a team has already marked a finding in a commonly used framework
as a false positive, other teams can may receive the benefit of not having to triage
the same finding.

As an example, the following ignore list "includes" two other ignore lists. The first
defines known false positives inside of a development framework that the project is
using, and the second defines suppressions specific to the container image which the
project is built using:

**stacs.ignore.json**
```json
{
    "include": [
        "framework.ignore.json",
        "container.ignore.json"
    ],
    "ignore": []
}
```

**framework.ignore.json**
```json
{
    "include": [],
    "ignore": [
        {
            "pattern": ".*/tests/.*",
            "reason": "Test fixtures contain example credentials."
        }
    ]
}
```

**container.ignore.json**
```json
{
    "include": [],
    "ignore": [
        {
            "pattern": "usr/lib/x86_64-linux-gnu/libgnutls\\.so\\.[0-9.]+",
            "reason": "TICKET-XYZ - GnuTLS contains example RSA and DSA keys.",
            "references": [
                "CredentialPKIPEMRSA",
                "CredentialPKIDSAPEM"
            ]
        }
    ]
}
```

Currently, STACS only supports inclusion of ignore lists by local file path, but there
are plans to allow remote ignore lists in future.

### Entries
{: .fs-6 }

STACS supports a number of different entry types in ignore lists. These allow users to
build ignore list entries which are as coarse as "ignore anything in this directory" to
"ignore SHA256 hash findings inside of a file with an MD5 sum of `abcd...`".

#### Supported Types
{: .fs-5 }

In order for ignore lists to be as flexible as possible, STACS allows a number of ways
to identify which findings to suppress, including:

* By MD5 hash.
* By file path (as a [Regular Expression](https://en.wikipedia.org/wiki/Regular_expression)).
* By rule reference.
* By finding offset (in bytes).

STACS allows these types to be combined to provide fine-grained control over
suppressions, in order to try and prevent accidental suppression of future issues.
Please see the appropriate sections below for more information on each of these types.

#### By MD5 hash
{: .fs-5 }

The 'key' for MD5 hash ignores in an ignore list entry is `md5`. The value must be a
string.

MD5 based suppressions are intended to be used to ignore a finding in an exact version
or copy of a file. This is used when a suppression should only be applied at a given
point in time, rather than "forever".

As an example, if a binary compiled for a project contained example credentials hash in
a particular build due to an error, then the user may only want to suppress findings
for this specific build, and not suppress all findings for this file in future.

In this case, the user could define an ignore list which suppresses this specific
version of the file using its MD5 hash:

```json
{
    "include": [],
    "ignore": [
        {
            "md5": "e95348ed81f439d0a73a18835bd78eec",
            "reason": "TICKET-XYZ - Ignore hashes accidentally included in v0.1.0"
        }
    ]
}
```

#### By file path
{: .fs-5 }

The 'key' for file path ignores in an ignore list entry is `pattern`. The value must be
a string.

File path suppressions allow findings in a file path, in a specific file, to be
suppressed. In order to allow flexibility in ignore list entries, this path must be
defined as a [regular expression](https://en.wikipedia.org/wiki/Regular_expression).

This ignore type is useful when findings inside of a directory of files should be 
suppressed, such as those which may be used only for testing.

```json
{
    "include": [],
    "ignore": [
        {
            "pattern": ".*/tests/.*",
            "reason": "TICKET-XYZ - Test fixtures contain cryptographic identifiers"
        }
    ]
}
```

#### By rule reference
{: .fs-5 }

The 'key' for rule reference ignores in an ignore list entry is `references`. The value
must be an array of strings.

Rule reference suppressions allow a user to suppress all findings from a specific rule
or set of rules. Although this may be useful in some cases on its own, this type is
usually used along side other ignore list entry types.

As an example, a user may want to suppress only `CredentialNixShadowBlank` findings from
`/etc/shadow` in a Docker image built using Alpine Linux due to this
[being a known issue](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5021)
which is has been confirmed exploitable in this particular build of a product:

```json
{
    "include": [],
    "ignore": [
        {
            "pattern": ".*/etc/shadow$",
            "reason": "TICKET-XYZ - CVE-2021-5021 is not exploitable in this build.",
            "references": ["CredentialNixShadowBlank"]
        }
    ]
}
```

#### By finding offset
{: .fs-5 }

The 'key' for offset ignores in an ignore list entry is `offset`. The value must be
an integer.

This is an advanced option which allows a user to suppress a finding which appears at
a specific location in a file (in bytes). Although this may be useful in some cases on
its own, this type is usually used along side other ignore list entry types.
