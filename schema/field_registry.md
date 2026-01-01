# Field Registry (Canonical) — Revenue Runway

This registry is the authoritative list of fields required to implement Revenue Runway in any CRM.
It includes object placement, field type, and (where applicable) allowed values.

---

## CONTACT OBJECT

### Identity (standard CRM fields)
- First Name (standard)
- Last Name (standard)
- Email (standard)
- Phone (standard)

### Structural Fields
#### Contact Type
- Type: Single-select dropdown
- Allowed values:
  - Lead
  - Customer
  - Partner
  - Partner Customer
  - Contractor
  - Vendor
  - Media / Press
  - Advisor
  - Internal
  - Other

#### Contact Role
- Type: Multi-select dropdown
- Allowed values:
  - Primary Decision Maker
  - Economic Buyer
  - Champion
  - Influencer
  - Technical Buyer
  - End User
  - Billing / Finance
  - Legal / Compliance
  - Procurement
  - Operations
  - Former Contact
  - Unknown

#### Re-Engagement Eligibility
- Type: Single-select dropdown
- Allowed values:
  - Yes – Primary
  - Yes – Secondary
  - No – Do Not Re-engage
  - Unknown

### Attribution + Campaign Tracking (Supporting Fields)
#### Source Page
- Type: Single line text
- Notes: full path or page identifier that created the contact/opportunity (e.g., /runway/accounting)

#### UTM Source
- Type: Single line text

#### UTM Medium
- Type: Single line text

#### UTM Campaign
- Type: Single line text

#### UTM Content
- Type: Single line text

#### Contact Original Source
- Type: Single-select dropdown
- Allowed values:
  - Cold Outreach
  - Organic Social Media
  - Paid Social Media
  - Social Media DM
  - Email Campaign
  - Podcast
  - Speaking Event
  - Trade Event
  - Referral Partner
  - Key Referral Partner
  - Networking / Other
  - Paid Ad
  - Website
  - Other

#### Outbound Campaign
- Type: Multi-select dropdown
- Allowed values: controlled naming convention (free text values permitted)
- Naming recommendation: RR-<Motion>-<Industry>-<YYYYQ#> (example: RR-Outbound-Accounting-2026Q1)

### Social Profile Supporting Fields (optional but recommended)
- LinkedIn Profile URL (single line text)
- Facebook Profile URL (single line text)
- Alignable Profile URL (single line text)
- Other Social URL (single line text)

### Domain Mirror (for account matching)
#### Company Domain (Contact)
- Type: Single line text
- Notes: mirrored from Company Primary Company Domain OR captured from form input; used for searching/segmentation when association tools are weak.

---

## COMPANY / ACCOUNT OBJECT

### Account Identity
- Company Name (standard)

### Matching + ABM
#### Primary Company Domain
- Type: Single line text
- Notes: canonical account key used for deduplication and contact association.

#### ABM Tier
- Type: Single-select dropdown
- Allowed values:
  - Tier 1
  - Tier 2
  - Tier 3
  - Not Rated
- Definitions: see definitions/abm_tiers.md

#### Account Lifecycle Stage
- Type: Single-select dropdown
- Allowed values:
  - Target
  - Engaged
  - Active Opportunity
  - Customer
  - Dormant
  - Former Customer
  - Disqualified

### Partner Relationship (Company-level)
#### Partner Type
- Type: Single-select dropdown
- Allowed values:
  - Referral Partner
  - Affiliate
  - Channel Partner
  - Strategic Partner
  - Co-Marketing Partner
  - Investor
  - Vendor
  - Community Partner
  - Other
- Definitions: see definitions/partner_types.md

---

## OPPORTUNITY / DEAL OBJECT

### Opportunity Classification
#### Opportunity Source
- Type: Single-select dropdown
- Allowed values:
  - Inbound – Marketing
  - SDR Qualified
  - Outbound – Sales
  - Partner – Referral
  - Partner – Channel / Reseller
  - Affiliate
  - Internal / Employee
  - Expansion / Upsell
  - Other / Unknown
- Definitions/usage: see schema/opportunity_source.md

### Velocity Anchors (Authoritative Dates)
#### Deal Entered MQL Date
- Type: Date
- Notes: set only if opportunity enters MQL pipeline

#### Deal Entered SQL Date
- Type: Date
- Notes: set when opportunity enters SQL pipeline (authoritative for rep velocity)

(Deal Created Date is system-generated and not used for velocity calculations.)
- See schema/deal_velocity_model.md

### Pipelines
- MQL stages: pipelines/mql_pipeline.md
- SQL stages: pipelines/sql_pipeline.md
