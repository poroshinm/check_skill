---
name: check
description: Classify suspicious files, repos, binaries, and archives, then triage them for malware indicators, secrets, obfuscation, and unsafe execution behavior. Use when an untrusted download, script, installer, program, or repository needs a fast safety check.
---

# Check

## Overview

Use this skill for the first pass on untrusted artifacts. It combines file-type classification with malicious-behavior triage so you can quickly tell what something is and whether it looks risky.

## Workflow

1. Identify the artifact type with `magika` and `file`.
2. Decide whether it is source, binary, archive, document, or mixed.
3. Inspect the attack surface without executing anything.
4. Run the narrowest static checks that fit the artifact.
5. Report what looks suspicious, what is merely unusual, and what needs sandboxing or deeper reverse engineering.

## Safety Rules

- Do not execute unknown code or open files in native apps just to "see what happens".
- Do not modify the original sample. Work on a copy.
- Do not upload samples to third-party services unless the user asks.
- Treat filenames, extensions, and README claims as untrusted.
- If dynamic analysis is needed, say so clearly.

## Source Repos

Check:

- install hooks and package scripts
- CI workflows and release pipelines
- submodules and vendored blobs
- obfuscated code, `eval`, base64 blobs, generated payloads
- network calls, shell execution, downloader behavior
- credential harvesting, secret exfiltration, clipboard access, persistence

Use `gitleaks` or `trufflehog` for secrets, `semgrep` for suspicious code patterns, and `rg` or `git grep` for manual review.

## Binaries, Archives, and Documents

Check:

- `magika` / `file` for type
- `strings`, `xxd`, `hexdump` for embedded text
- `binwalk` for packed or embedded payloads
- `exiftool` for metadata
- `yara` for rule-based matching
- `capa` for executable capabilities
- `clamscan` for signature-based scanning

If the sample looks packed or encrypted, say static analysis may be incomplete.

## Reporting

Return:

- verdict: low / medium / high risk
- why: 3 to 6 concrete indicators
- confidence: low / medium / high
- next step: static only, sandbox, or manual reverse engineering

Flag false positives separately. If you cannot verify something, say so.

## References

See [references/tools.md](references/tools.md) for command suggestions and a decision table.
