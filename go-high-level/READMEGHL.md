# GoHighLevel — Platform Implementation Notes

This folder contains GoHighLevel-specific documentation, schemas, and
workflow installation artifacts.

GoHighLevel limitations that affect architecture:
- No required fields at the Opportunity level
- Limited native enforcement of stage rules
- Surveys are used to enforce structured data capture
- Workflows must intercept stage changes to enforce compliance

## Source of Truth
- Canonical fields, enums, and pipelines are defined in Google Sheets
- GitHub contains implementation patterns and installer artifacts
- GHL is the execution layer

## Folder Structure
- /schemas.md — how fields and schemas are handled in GHL
- /workflows — installable GHL workflows (each isolated)
