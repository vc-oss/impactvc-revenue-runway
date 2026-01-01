# Schemas in GoHighLevel

Fields and enums are defined and maintained in Google Sheets.

This repo does NOT duplicate field definitions.

## Patterns
- Boolean completion flags are required for enforcement workflows
- Surveys are used to capture required structured data
- Survey submissions write back to Opportunity fields

## Platform Notes
- GHL does not support required Opportunity fields
- Survey fields must be mapped manually
- Opportunity association must be preserved in survey links
