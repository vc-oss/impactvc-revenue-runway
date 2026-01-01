# WF-001 — Domain Matching & Mirroring

## Purpose
Use domain as the canonical key for account-level tracking and re-engagement.
Ensure contacts carry a usable domain value even if CRM account association is weak.

---

## Inputs
- Company: Primary Company Domain (canonical)
- Contact: Company Domain (mirror)
- Contact: Email (for fallback domain parsing)

---

## Normalization Rules (applies to any domain string)
When setting either domain field, normalize:
- lowercase
- remove leading "http://", "https://"
- remove leading "www."
- remove trailing "/" and path
Example:
  https://www.Example.com/about -> example.com

---

## Behavior

### A) When a NEW Contact is created/updated
1) If Contact.Company Domain is present:
   - normalize it
2) Else if Email domain is business domain (not gmail/yahoo/outlook/etc):
   - set Contact.Company Domain = normalized email domain
3) Else:
   - leave blank (requires manual fill later)

### B) When a Company Primary Company Domain is set/updated
- normalize Company.Primary Company Domain
- for all associated contacts (or contacts matched by domain if association is limited):
  - set Contact.Company Domain = Company.Primary Company Domain (only if contact field is empty OR differs)

### C) When an Opportunity is created (optional but recommended)
- copy Contact.Company Domain into Opportunity/notes or use it for routing logic (depends on CRM capability)

---

## Notes for GoHighLevel
If CRM cannot reliably update all contacts under a company, use a fallback:
- segment contacts by Contact.Company Domain = Company.Primary Company Domain
- apply updates in batch workflows where possible
