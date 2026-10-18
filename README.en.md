# cn-data-class-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-dat%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Standard: GB/T 43697](https://img.shields.io/badge/standard-GB%2FT%2043697--2024-2ea44f.svg)](domain.manifest.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **Status: RESERVED (placeholder · open for claim)** — this repository ships the four-piece industry-access skeleton already built to the platform protocol, and waits for an operator with the proper domain qualification to claim it and fill in the real rules.

It turns "which class and grade is this data, and which protection obligations follow" into verifiable attributes: the AI only surfaces the elements — it does **not** issue a final grading.

## What this domain covers

Data classification, grading and protection-obligation determination (China context, GB/T 43697). Classifying data as general / important / core directly sets protection intensity and cross-border requirements. The AI lays out the elements; final grading stays with the data owner.

## Quick start

This repo is a **domain-profile skeleton** (four JSON files), not a runnable application. Validate with standard JSON tooling:

```bash
git clone https://github.com/SynomosAI/cn-data-class-checker.git
cd cn-data-class-checker
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json      > /dev/null
```

The checker (MCP tool) IO contract is in `checker.io.json`. A minimal call:

```json
{
  "subject_ref": "mpp:cn:8d1e2f3a",
  "attribute_keys": ["dat.data_class", "dat.cross_border_obligation"],
  "evidence": ["tsa:2026-09-12#sha256:7b8c..."],
  "request_id": "req-20260912-0005"
}
```

> The checker has exactly one action — *show and verify*. It has **no "judge" option**: "cross-border conditions already met", "important-data grade", "may cross border" conclusions are all on the `_forbidden_outputs` blacklist.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `dat` (globally unique; never reused) |
| Domain name | Data · Classification/grading & protection obligations (China context) |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Placeholder since | 2026-09-12 |

## Attributes

| Key | Type | Meaning |
|---|---|---|
| `dat.data_class` | enum | Per GB/T 43697-2024 · general/important/core/undetermined |
| `dat.sensitivity` | enum | Sensitivity tier for access control and display · public/internal/confidential/secret |
| `dat.contains_personal_info` | boolean | Whether personal information is present (stacks PIPL obligations) |
| `dat.cross_border_obligation` | enum | Cross-border path obligation, human-confirmed · none/contract_required/security_assessment/certification/prohibited |
| `dat.retention_policy` | string | Retention period and expiry disposition |

## Red lines (machine-readable)

1. Identification of important and core data requires human confirmation by the data owner; the AI must not grade on its own.
2. No "cross-border conditions already met" wording; cross-border follow statutory paths.
3. No final data-grading conclusion — only element checks and obligation hints.

Each red line has a corresponding blocking rule in `gate-map.json`.

## The four-piece access set

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attribute list, issuer requirements, validity, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy declaration: default-off, minimal, withdrawable, deletable |
| `checker.io.json` | Checker (MCP tool) IO contract — show & verify, never judge |

## References

GB/T 43697-2024 Data security — Data classification and grading rules; Data Security Law of the PRC; Personal Information Protection Law of the PRC; Measures for Security Assessment of Outbound Data. References only explain the source of attributes; they do not constitute legal advice.

## How to claim

1. Fork this repo and fill in `operator` and `checker_endpoint`.
2. Calibrate attribute values and red-line wording per currently effective rules.
3. Run the platform-side validator self-check (all five criteria must pass).
4. Open a PR with qualification evidence and the rule basis.

## License / 署名

Code and configurations are used under the MIT licence. Document copyright belongs to SynomosAI.

© 2026 SynomosAI. All rights reserved.
