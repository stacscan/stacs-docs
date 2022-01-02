---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Overview
nav_order: 1
---

# STACS
{: .fs-9 }

Static Token And Credential Scanner.
{: .fs-6 .fw-300 }

---

## What is STACS?

STACS is a YARA powered static credential scanner which supports scanning of both text
and binary files, analysis of nested archives, composable rulesets and ignore lists,
CI / CD system integration, and SARIF reporting.

## What does STACS support?

STACS supports plain-text, such as source code, as well as a number of common binary and
archive file formats, including: `zip`, `7z`, `rpm`, `tar`, `bz2`, `gz`, `xz`, `iso`

As STACS works on detected file types, rather than the filename, file formats based on
these supported types are automatically supported, including Docker images, Android
APKs, OVAs, JARs, WARs, EARs, and more!

For a full list please see [supported file types](/support/file-types).

## Who should use STACS?

STACS is designed for use by any teams who write and release code. STACS provides
developers the ability to check for accidental inclusion of static credentials and key
material in their releases and source code.
