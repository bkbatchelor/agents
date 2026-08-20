---
name: software-support-auditor
description: Expert AI Software Support Quality Assurance Auditor. Scans codebases to identify production error sites, exceptions, and failure modes, mapping each issue to its required support tier (L0 through L4) and generating dual-format audit reports.
version: 1.0.0
author: Enterprise Software Support Engineering
keywords:
  - software-support
  - error-scan
  - support-tier-mapping
  - code-audit
  - l0-l4-classification
  - stts
  - iso-20000
  - itsm
triggers:
  - audit codebase
  - scan production errors
  - map support tiers
  - software support scan
  - "@software-support-auditor"
---

# Kiro Power: Software Support Auditor (`software-support-auditor`)

This Kiro CLI Power activates an expert **Software Support Quality Assurance & Codebase Auditor**. It inspects target repositories, source code files, API handlers, and middleware to identify runtime failure modes and map them directly to support tiers (**Level 0 through Level 4**).

The underlying governance framework is directly grounded in enterprise support standards:
- **ISO/IEC 20000-1:2018** (Service Management & Incident/Problem Processes)
- **ISO/IEC/IEEE 12207:2017** (Software Life Cycle Processes)
- **AICPA SOC 2 Type II** (CC7.2–CC7.4 Incident Management)
- **Structured Ticket Transfer Specification (STTS)**

---

## Usage in Kiro CLI

Activate this power in Kiro CLI using any of the following invocations:

```bash
# Invoke power directly
@software-support-auditor scan codebase

# Or trigger via natural language prompt
kiro "Audit this codebase for production errors and map them to L0-L4 support tiers using software-support-auditor"
```

---

## Power Capabilities

1. **Automated Error Discovery:** Scans codebases for HTTP status codes (`4xx`/`5xx`), custom exception definitions, error enumerations, throw/raise statements, and logger error calls.
2. **Support Tier Mapping (L0–L4):**
   - **Level 0 (L0 / AI Self-Service):** Password expiration, account lockout, self-service workflows.
   - **Level 1 (L1 / Service Desk):** Client validation errors (`400`, `401`), invalid form inputs, file size limit exceeded.
   - **Level 2 (L2 / App Support):** Permission sync (`403`), resource missing (`404`), state conflict (`409`), database deadlocks, transient API timeouts.
   - **Level 3 (L3 / Engineering):** Unhandled runtime exceptions (`500`), null pointer errors, segmentation faults, out of memory, source code defects.
   - **Level 4 (L4 / Vendor / OEM):** Bad gateway (`502`), service unavailable (`503`), cloud infrastructure access denied (e.g., `AWS_S3_ACCESS_DENIED`), third-party SDK panics.
3. **Mandatory Dual Output:**
   - **Section 1:** Executive Codebase Error Catalog Report (Markdown).
   - **Section 2:** Machine-Readable ITSM Inspection Payload (JSON matching `examples/audit_output_schema.json`).

---

## Files Included in This Power

- `STEERING.md` — Agent instruction rules and scanning workflow.
- `rules/codebase_scanning_rules.md` — Pattern regexes and L0–L4 tier assignment matrix.
- `templates/error_catalog_template.md` — Template for human-readable audit reports.
- `templates/audit_output_schema.json` — JSON schema for machine-readable payloads.
