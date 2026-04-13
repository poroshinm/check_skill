# Tools and Checks

## Quick Decision Table

| Artifact | First pass | Follow-up |
|---|---|---|
| Repo / source tree | `magika -r`, `file`, `git ls-files`, `rg -n` for `curl`, `wget`, `Invoke-WebRequest`, `powershell`, `bash -c`, `eval`, `exec`, `system`, `subprocess`, `base64`, `chmod +x` | `gitleaks`, `trufflehog`, `semgrep` |
| Binary / executable | `magika`, `file`, `sha256sum`, `strings` | `binwalk`, `yara`, `capa`, `clamscan` |
| Archive / document | `magika`, `file`, `unzip -l`, `7z l`, `strings`, `exiftool` | `binwalk`, `yara`, `clamscan` |
| Mixed repo + binaries | Do both source and binary checks | Isolate suspicious files for deeper analysis |

## Repo Checks

- Inspect install surfaces: `package.json`, `requirements.txt`, `pyproject.toml`, `Cargo.toml`, `Gemfile`, `Makefile`, `Dockerfile`, `.github/workflows/`
- Look for download-and-execute behavior, persistence, credential access, and obfuscation
- Check for secrets with `gitleaks` or `trufflehog`
- Use `semgrep` when the repo is code and you want pattern-based security checks

## Binary Checks

- `magika sample`
- `file sample`
- `strings -a sample | less`
- `binwalk sample`
- `exiftool sample`
- `yara rules.yar sample`
- `capa sample`
- `clamscan sample`

## Red Flags

- Unexpected network access
- Shell execution or downloaders
- Base64 or hex blobs with runtime decoding
- Packed or encrypted payloads
- Embedded scripts, macros, or command strings
- Secrets, tokens, private keys, or browser/session theft behavior

## Reporting Template

```text
Verdict: low / medium / high risk
Indicators:
- ...
- ...
Confidence: low / medium / high
Next step: static only / sandbox / manual reverse engineering
```
