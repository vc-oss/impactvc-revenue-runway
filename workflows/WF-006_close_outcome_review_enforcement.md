# WF-006 — Close Outcome Review Enforcement (Blocking)

WF-006 enforces clean **Close Won / Close Lost** data by intercepting close attempts and
requiring completion of a structured Win/Loss Review Survey (prefilled where possible).

**WF-006 does NOT replace WF-002.**  
WF-002 remains lifecycle date stamping (MQL/SQL). WF-006 is close outcome enforcement.

---

## Purpose

1. Prevent deals from being finalized as **Closed Won** or **Closed Lost** without required win/loss intelligence.
2. Avoid reliance on rep tasks/reminders by using **manager-visible Pending Review stages**.
3. Ensure Win/Loss data is consistently captured for reporting and coaching.
4. Keep the workflow portable across CRMs via explicit fields and surveys/playbooks.

---

## Dependencies

### Pipeline Stages (must exist)
- `Pending Win Review`
- `Pending Loss Review`
- `Closed Won`
- `Closed Lost`

### Opportunity Fields (canonical)
**Win**
- `won_reasons` (Multi-select; enum `WON_REASON`)
- `won_description` (Multi-line text)
- `win_review_completed` (Yes/No)
- `win_review_completed_date` (Date) *(optional but recommended)*

**Loss**
- `rr_loss_reason` (Single-select; enum `RR_LOSS_REASON`)
- `loss_description` (Multi-line text) *(optional but recommended)*
- `loss_review_completed` (Yes/No)
- `loss_review_completed_date` (Date) *(optional but recommended)*

### Survey Assets (must exist)
- `Revenue Runway — Win Review` (Survey)
- `Revenue Runway — Loss Review` (Survey)

---

## Workflow Triggers (4 Total)

1) **Intercept Closed Won Attempt**  
2) **Intercept Closed Lost Attempt**  
3) **Win Review Survey Submitted**  
4) **Loss Review Survey Submitted**

---

## Trigger 1 — Intercept Closed Won Attempt

### Trigger
- Opportunity stage changes to: `Closed Won`

### Conditions
- IF `win_review_completed` is NOT `Yes`

### Actions
1. Move Opportunity stage to: `Pending Win Review`
2. Notify Opportunity Owner with the Win Review Survey link:
   - Message: "Win review required to finalize Closed Won. Complete the Win Review survey."
3. (Optional) Add internal note to Opportunity timeline:
   - "WF-006: Win review required. Routed to Pending Win Review."

### Guardrails
- Do NOT overwrite `won_reasons` or `won_description` here.
- Do NOT set `win_review_completed` here.

---

## Trigger 2 — Intercept Closed Lost Attempt

### Trigger
- Opportunity stage changes to: `Closed Lost`

### Conditions
- IF `loss_review_completed` is NOT `Yes`

### Actions
1. Move Opportunity stage to: `Pending Loss Review`
2. Notify Opportunity Owner with the Loss Review Survey link:
   - Message: "Loss review required to finalize Closed Lost. Complete the Loss Review survey."
3. (Optional) Add internal note to Opportunity timeline:
   - "WF-006: Loss review required. Routed to Pending Loss Review."

### Guardrails
- Do NOT overwrite `loss_reason` or `loss_description` here.
- Do NOT set `loss_review_completed` here.

---

## Trigger 3 — Win Review Survey Submitted → Finalize Closed Won

### Trigger
- Survey submitted: `Revenue Runway — Win Review`

### Required Survey-to-Field Mapping
- Survey: Won Reasons → `won_reasons`
- Survey: Won Description → `won_description`
- (Optional) Survey: Perfect Prospect Playbook Score → `ppm_playbook_score`
- (Optional) Survey: Perfect Prospect Playbook Notes → `ppm_playbook_notes`

### Actions
1. Update Opportunity fields from survey answers:
   - `won_reasons`
   - `won_description`
   - `ppm_playbook_score` *(optional)*
   - `ppm_playbook_notes` *(optional)*
2. Set `win_review_completed` = `Yes`
3. Set `win_review_completed_date` = `Today` *(optional but recommended)*
4. Move Opportunity stage to: `Closed Won`
5. (Optional) Add internal note:
   - "WF-006: Win review completed. Deal finalized Closed Won."

### Guardrails
- Ensure the survey submission is associated to the correct Opportunity.
- Do NOT finalize if required survey questions are missing (survey should enforce required fields).

---

## Trigger 4 — Loss Review Survey Submitted → Finalize Closed Lost

### Trigger
- Survey submitted: `Revenue Runway — Loss Review`

### Required Survey-to-Field Mapping
- Survey: Loss Reason → `loss_reason`
- Survey: Loss Description → `loss_description` *(optional)*

### Actions
1. Update Opportunity fields from survey answers:
   - `loss_reason`
   - `loss_description` *(optional)*
2. Set `loss_review_completed` = `Yes`
3. Set `loss_review_completed_date` = `Today` *(optional but recommended)*
4. Move Opportunity stage to: `Closed Lost`
5. (Optional) Add internal note:
   - "WF-006: Loss review completed. Deal finalized Closed Lost."

### Guardrails
- Ensure the survey submission is associated to the correct Opportunity.
- Do NOT finalize if required survey questions are missing (survey should enforce required fields).

---

## Implementation Notes (GHL)

- GHL does not support “required fields” at the Opportunity object level.
- Enforcement is achieved by:
  - intercepting close attempts
  - routing to Pending Review stages
  - using required survey questions
  - writing answers back to Opportunity fields on submission

**Manager Visibility**
- Managers can monitor deals stuck in:
  - `Pending Win Review`
  - `Pending Loss Review`

**Founder-led Sales**
- WF-006 may be implemented later if enforcement adds friction during early founder-led selling.
- Schema should still include the fields and surveys for future readiness.

---
