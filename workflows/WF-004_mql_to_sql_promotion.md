# WF-004 — MQL → SQL Promotion (ICP Qualified)

## Purpose
Promote opportunities from MQL to SQL in a controlled, reportable way that:
- preserves velocity anchors
- supports SDR handoff
- prevents accidental promotion

---

## Dependencies
- pipelines/mql_pipeline.md
- pipelines/sql_pipeline.md
- workflows/WF-002_velocity_date_stamping.md

---

## Trigger Stage (MQL)
- "ICP Qualified – Move to SQL Pipeline"

This is an exit stage used as an intentional handoff signal.

---

## Automation — Promotion to SQL
### Trigger
Opportunity stage changes to:
- ICP Qualified – Move to SQL Pipeline (in MQL pipeline)

### Conditions
- Account/Company is ICP Yes (or equivalent signal)
- Optional: a meeting is set or completed (implementation choice)
- Optional: required fields are present (email or company domain)

### Actions (in order)
1) Move Opportunity to SQL Pipeline
2) Set SQL Stage = Discovery (default starting stage)
3) If Deal Entered SQL Date is empty:
   - Set Deal Entered SQL Date = Today
4) (Optional but recommended) Set Company Account Lifecycle Stage = Active Opportunity
5) (Optional) Assign owner to AE / sales queue
6) (Optional) Add internal note: "Promoted from MQL by SDR on <date>"

---

## Guardrails
- Do not allow promotion from any other MQL stage
- Do not overwrite Deal Entered SQL Date if already set (late-entry protection)
- If an opportunity is moved back to MQL, do not clear Deal Entered SQL Date
