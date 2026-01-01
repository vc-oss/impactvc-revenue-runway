# WF-003 — Opportunity Source (Set Once)

## Purpose
Ensure every opportunity has an Opportunity Source that:
- identifies originating motion (inbound, SDR, sales outbound, partner, etc.)
- is stable for fair reporting
- is not overwritten by downstream automation

Opportunity Source is not the same as contact source or UTMs.

---

## Required Field (Opportunity / Deal)
- Opportunity Source (single-select dropdown)

Allowed values are defined in:
- schema/opportunity_source.md
- schema/field_registry.md

---

## Rules (Global)
- Opportunity Source must be set once, early
- Automation may set it when missing
- Automation must never overwrite an existing value
- Manual correction is allowed only as a data correction

---

## Automation A — Inbound Creation
### Trigger
Opportunity created from inbound forms / bookings / website flows

### Conditions
- Opportunity Source is empty

### Actions
- Set Opportunity Source = Inbound – Marketing

---

## Automation B — SDR-created Opportunity
### Trigger
Opportunity created by SDR workflow, SDR user, or SDR pipeline entry

### Conditions
- Opportunity Source is empty

### Actions
- Set Opportunity Source = SDR Qualified

---

## Automation C — Sales-created / Outbound Opportunity
### Trigger
Opportunity created by sales user OR created directly in SQL pipeline

### Conditions
- Opportunity Source is empty

### Actions
- Set Opportunity Source = Outbound – Sales

---

## Automation D — Partner-sourced Opportunity (Referral / Channel / Affiliate)
### Trigger
Opportunity created with partner indicators (implementation-specific):
- partner referral form
- partner selection field
- partner tag
- affiliate attribution

### Conditions
- Opportunity Source is empty

### Actions (choose one based on signal)
- Partner – Referral
- Partner – Channel / Reseller
- Affiliate

---

## Automation E — Expansion / Upsell
### Trigger
Opportunity created from an existing customer account motion

### Conditions
- Opportunity Source is empty

### Actions
- Set Opportunity Source = Expansion / Upsell

---

## Guardrail — Do Not Overwrite
### Trigger
Any update to opportunity

### Conditions
- Opportunity Source is not empty

### Actions
- No action (do not overwrite)
