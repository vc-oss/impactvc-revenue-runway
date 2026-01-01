# WF-002 — Velocity Date Stamping (MQL + SQL)

## Purpose
Stamp authoritative dates that enable fair velocity reporting even when:
- deals are entered late
- stages are moved backwards
- stages are skipped

Revenue Runway uses two authoritative dates:
- Deal Entered MQL Date (marketing-owned)
- Deal Entered SQL Date (sales-owned)

These are distinct from system-created date and stage-change timestamps.

---

## Required Fields (Opportunity / Deal)
- Deal Entered MQL Date (date)
- Deal Entered SQL Date (date)

---

## Rules (Global)
- Set each date only once (never overwrite once populated)
- Allow manual backdating as a data correction (optional governance)
- Do not derive sales performance from Deal Created Date

---

## Automation A — MQL Entry Date Stamp
### Trigger
Opportunity enters the MQL pipeline (any stage)

### Conditions
- Deal Entered MQL Date is empty

### Actions
- Set Deal Entered MQL Date = Today

### Notes
- This stamps the first moment the opportunity became marketing/SDR-managed.
- It is allowed to remain blank for sales-created opportunities that never enter MQL.

---

## Automation B — SQL Entry Date Stamp (Authoritative Sales Anchor)
### Trigger
Opportunity enters the SQL pipeline (any stage)

### Conditions
- Deal Entered SQL Date is empty

### Actions
- Set Deal Entered SQL Date = Today

### Notes
- This stamps when the opportunity became sales-owned.
- This date is the required anchor for rep velocity calculations.

---

## Automation C — Guardrail (Do Not Overwrite)
### Trigger
Any update to opportunity pipeline/stage

### Conditions
- Deal Entered MQL Date is not empty OR Deal Entered SQL Date is not empty

### Actions
- No action (explicitly do not overwrite)

---

## Reporting Guidance
### Sales Velocity
Deal Entered SQL Date → Close Won / Close Lost

### Marketing/SDR Velocity
Deal Entered MQL Date → Deal Entered SQL Date

### Full Funnel Velocity (if desired)
Deal Entered MQL Date → Close Won / Close Lost
