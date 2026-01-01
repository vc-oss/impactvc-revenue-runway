# Workflows

This folder contains **CRM-agnostic workflow specifications** used by the
Revenue Runway operating system.

Workflows define **behavior and enforcement rules** that operate on top of:
- definitions/ (business meaning)
- schema/ (fields and data contracts)
- pipelines/ (stage progression)

They are written to be portable across CRMs (GoHighLevel, HubSpot, Salesforce, etc.)
and intentionally avoid tool-specific screenshots or UI instructions.

---

## Design Principles

- Workflows enforce rules; they do not redefine meaning
- Automation must never overwrite authoritative data once set
- Human behavior is assumed to be imperfect (late entry, stage backsliding)
- Fair reporting and accountability take precedence over rigid process

---

## Workflow Index

### WF-001 — Domain Matching & Mirroring
Ensures company-level domain is the canonical key and is mirrored to contacts
for reliable account-based segmentation and re-engagement.

File:
- WF-001_domain_matching.md

---

### WF-002 — Velocity Date Stamping
Stamps authoritative MQL and SQL entry dates used for velocity reporting,
independent of record creation date or stage movement.

File:
- WF-002_velocity_date_stamping.md

---

### WF-003 — Opportunity Source (Set Once)
Ensures every opportunity has a stable, non-overwritable Opportunity Source
that fairly attributes origin without political bias.

File:
- WF-003_opportunity_source_set_once.md

---

### WF-004 — MQL → SQL Promotion
Controls intentional promotion from MQL to SQL via the
"ICP Qualified – Move to SQL Pipeline" stage.

File:
- WF-004_mql_to_sql_promotion.md

---

### WF-005 — Company Re-engagement Orchestration
Defines how to re-engage companies while targeting only the correct contacts
based on role, eligibility, and ABM tier.

File:
- WF-005_company_reengagement.md

---

## Implementation Notes

- Each workflow may be implemented using native automation, API, or batch processes
  depending on CRM capabilities.
- Workflow numbering is stable and should not be reused.
- Changes to workflow logic should be versioned and documented.
