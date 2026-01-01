# WF-006 — Close Outcome Review Enforcement (GoHighLevel)

WF-006 enforces completion of a Win or Loss Review before an opportunity
can be finalized as Closed Won or Closed Lost in GoHighLevel.

This workflow exists because GoHighLevel cannot enforce required fields
at the Opportunity level.

## What this workflow does
- Intercepts attempts to move an opportunity to Closed Won or Closed Lost
- Routes the deal to a Pending Review stage if required data is missing
- Uses required surveys to collect win/loss intelligence
- Finalizes the deal only after survey submission

## Why Pending Review stages exist
Pending Review stages are **blocking compliance stages**, not selling stages.
They provide manager visibility into deals that are functionally closed
but missing required review data.

## Dependencies (must exist before install)
- SQL pipeline with the following stages:
  - Pending Win Review
  - Pending Loss Review
  - Closed Won
  - Closed Lost
- Win Review Survey: `Revenue Runway — Win Review`
- Loss Review Survey: `Revenue Runway — Loss Review`
- Canonical opportunity fields defined in Google Sheets

## Installation
1. Confirm pipeline stages exist in the SQL pipeline
2. Create the Win and Loss Review surveys
3. Paste the contents of `wf-006.prompt.md` into the GHL Workflow AI Builder
4. Review each trigger and action for accuracy
5. Activate the workflow

## Source of truth
- Field definitions, enums, and schema locations are maintained in Google Sheets
- This README explains intent and behavior
- The GHL AI prompt is the executable artifact
