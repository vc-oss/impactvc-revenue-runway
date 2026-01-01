# WF-005 — Company Re-engagement Orchestration (ABM-safe)

## Purpose
Enable re-engagement at the company level while contacting only the right people.
This prevents spraying messages to technical contacts and focuses on the primary
buying environment.

---

## Required Fields
### Company
- Account Lifecycle Stage
- ABM Tier
- Primary Company Domain

### Contact
- Contact Role (multi-select)
- Re-Engagement Eligibility
- Company Domain (contact mirror)
- Contact Type

---

## Target Company Criteria (example)
A company qualifies for re-engagement when:
- Account Lifecycle Stage is Dormant OR Former Customer
AND
- ABM Tier is Tier 1 OR Tier 2
AND/OR
- Time since last engagement exceeds threshold (implementation-specific)

---

## Target Contact Criteria (who to contact)
Include contacts where:
- Re-Engagement Eligibility is Yes – Primary OR Yes – Secondary
AND
- Contact Role contains at least one:
  - Primary Decision Maker
  - Economic Buyer
  - Champion
  - Influencer
AND
- Contact Type is not:
  - Vendor
  - Contractor
  - Media / Press
  - Internal

Optional:
- Company Domain matches Primary Company Domain (for association robustness)

---

## Workflow Pattern (recommended)
### Step 1 — Build Contact Audience
- Identify target companies
- Pull eligible contacts under those companies (or domain-match)

### Step 2 — Launch Contact Motion
- Enroll eligible contacts into outbound nurture / re-engagement campaign
- Use Outbound Campaign field to record membership

### Step 3 — Outcomes
If a contact engages (reply/book/click threshold):
- Update Company Account Lifecycle Stage = Engaged
- (Optional) Create or move opportunity into MQL or SQL depending on motion

---

## Guardrails
- Never re-engage contacts marked No – Do Not Re-engage
- Never default to technical buyer unless explicitly marked eligible
- Do not change ABM Tier automatically during re-engagement
