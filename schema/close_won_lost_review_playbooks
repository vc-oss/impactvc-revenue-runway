# Close Won / Close Lost Review Playbooks (Blocking) — Source of Truth

This spec defines the **required** Win/Loss review mechanism used to enforce clean CRM data at the point of deal finalization.

It is designed to work in platforms that **cannot enforce required fields at the object level** (e.g., GoHighLevel) by using:
- a **required survey/playbook** (prefilled from CRM)
- a **blocking workflow intercept** (Pending Review stages)
- a **survey submission workflow** (writes back to CRM fields and finalizes stage)

This pattern is intentionally portable to HubSpot, Salesforce, and other CRMs.

---

## Goals

1. Prefill review forms from the current Opportunity record (reduce duplicate entry).
2. Require completion of missing win/loss intelligence before finalizing the deal.
3. Make compliance **visible** to managers without relying on rep tasks/reminders.
4. Preserve clean reporting for:
   - Win/Loss analysis
   - sales process adherence
   - attribution & velocity
   - coaching and enablement

---

## Definitions

- **Intercept**: Preventing Close Won/Lost by moving the deal to a "Pending Review" stage.
- **Review Completion Flag**: Boolean (or equivalent) field proving review is complete.
- **Playbook Survey**: A structured form that can prefill from Opportunity data and requires missing answers.

---

## Required Opportunity Fields (Canonical)

> Note: Field keys are canonical; platforms may vary in implementation details.

### Win Review Fields
- `won_reasons` (Multi-select enum `WON_REASON`)
- `won_description` (Multi-line text)
- `win_review_completed` (Yes/No boolean)
- `win_review_completed_date` (Date) — optional but recommended

### Loss Review Fields
- `loss_reason` (Single-select enum `LOSS_REASON`) *(or `loss_reasons` if you choose multi-select later)*
- `loss_description` (Multi-line text) — optional but recommended
- `loss_review_completed` (Yes/No boolean)
- `loss_review_completed_date` (Date) — optional but recommended

### Perfect Prospect Meeting Playbook (Optional / Future-Ready)
- `ppm_playbook_score` (Number)
- `ppm_playbook_notes` (Multi-line text)

---

## Pipeline Stages (Canonical)

You must have **review intercept stages** that are manager-visible.

### Required Stages
- `Pending Win Review`
- `Pending Loss Review`

### Final Stages
- `Closed Won`
- `Closed Lost`

> If your CRM requires separate pipelines, these stages must exist in each relevant pipeline where closing can occur.

---

## System Behavior Overview

### Close Won Attempt
1. Rep attempts to set stage = `Closed Won`
2. System checks `win_review_completed`
3. If incomplete:
   - moves deal to `Pending Win Review`
   - assigns Win Review Survey link
   - does NOT allow final close

### Win Review Submission
1. Rep submits Win Review Survey (prefilled where possible)
2. System writes survey answers to Opportunity fields
3. System sets:
   - `win_review_completed = Yes`
   - `win_review_completed_date = Today` (optional)
4. System moves deal to `Closed Won`

### Close Lost Attempt
Same as above using Loss fields/stages/survey.

---

## Enforcement Logic (Platform-Agnostic)

### A) Intercept Workflow — Close Won

**Trigger**
- Opportunity stage changes to `Closed Won` (attempt)

**Condition**
- IF `win_review_completed` != `Yes`

**Actions**
1. Set stage to `Pending Win Review`
2. Assign Win Review Survey link to rep (email/SMS/internal notification)
3. (Optional) Add note to Opportunity timeline: "Win review required to finalize Close Won."

**Notes**
- Do NOT rely on tasks alone.
- This intercept stage is the manager-visible enforcement mechanism.

---

### B) Intercept Workflow — Close Lost

**Trigger**
- Opportunity stage changes to `Closed Lost` (attempt)

**Condition**
- IF `loss_review_completed` != `Yes`

**Actions**
1. Set stage to `Pending Loss Review`
2. Assign Loss Review Survey link to rep
3. (Optional) Add note to Opportunity timeline: "Loss review required to finalize Close Lost."

---

### C) Survey Completion Workflow — Win Review

**Trigger**
- Win Review Survey submitted

**Actions**
1. Update Opportunity fields from survey responses:
   - `won_reasons`
   - `won_description`
   - `ppm_playbook_score` (optional)
   - `ppm_playbook_notes` (optional)
2. Set:
   - `win_review_completed = Yes`
   - `win_review_completed_date = Today` (optional)
3. Move Opportunity stage to `Closed Won`

**Guardrails**
- Do not overwrite if fields already exist unless the submission explicitly updates them.
- If your platform cannot reference the Opportunity directly, ensure the survey is launched from a context that associates it to the correct Opportunity (deal link).

---

### D) Survey Completion Workflow — Loss Review

**Trigger**
- Loss Review Survey submitted

**Actions**
1. Update Opportunity fields from survey responses:
   - `loss_reason`
   - `loss_description` (optional)
2. Set:
   - `loss_review_completed = Yes`
   - `loss_review_completed_date = Today` (optional)
3. Move Opportunity stage to `Closed Lost`

---

## Survey Specs (Canonical)

### 1) Win Review Survey — Required Questions

**Survey Name**
- `Revenue Runway — Win Review`

**Prefill Source**
- Opportunity record fields where supported.

**Questions**
1. **Won Reasons** *(Required)*
   - Type: Multi-select
   - Enum: `WON_REASON`
   - Maps to: `won_reasons`

2. **Won Description** *(Required)*
   - Type: Long text
   - Maps to: `won_description`
   - Prompt: "Write the win story like you're shouting from the rooftops. What actually made this deal close?"

3. **Perfect Prospect Meeting Playbook Used?** *(Optional / future)*
   - Type: Yes/No
   - Maps to: (optional) `ppm_playbook_used` *(only if you add this field)*

4. **Perfect Prospect Playbook Score** *(Optional)*
   - Type: Number (or calculated from checklist)
   - Maps to: `ppm_playbook_score`

5. **Perfect Prospect Playbook Notes** *(Optional)*
   - Type: Long text
   - Maps to: `ppm_playbook_notes`

> If you later implement a scored checklist, replace questions 3–5 with the checklist questions and calculate a score.

---

### 2) Loss Review Survey — Required Questions

**Survey Name**
- `Revenue Runway — Loss Review`

**Prefill Source**
- Opportunity record fields where supported.

**Questions**
1. **Loss Reason** *(Required)*
   - Type: Single-select
   - Enum: `LOSS_REASON`
   - Maps to: `loss_reason`

2. **Loss Description** *(Optional but recommended)*
   - Type: Long text
   - Maps to: `loss_description`
   - Prompt: "What was the true reason we lost? What would change the outcome next time?"

---

## HubSpot Implementation Notes (for later programming)

HubSpot can enforce required properties more directly through:
- Required fields per pipeline stage (Enterprise features)
- Deal stage rules
- Playbooks

Even in HubSpot, this survey-based pattern remains valuable because it:
- standardizes narrative capture
- enables consistent coaching
- keeps the experience uniform across CRMs

**Recommended HubSpot mapping**
- Create HubSpot Playbooks for Win/Loss Review aligned to the same fields and enums.
- Use stage rules where available, but keep this doc as the canonical logic.

---

## Reporting Outcomes

This spec enables clean reporting on:
- Win reasons (multi-causal)
- Loss reasons
- Sales process adherence (PPM score)
- Manager visibility (Pending Review stages)
- Rep fairness (structured outcomes, not subjective notes)

---

## Status

- Spec: **Canonical**
- Automation: **Platform-specific implementation required**
- Recommended Phase: Enforce after initial pipeline adoption or when reps are involved
