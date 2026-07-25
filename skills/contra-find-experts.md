---
name: Find and filter experts in a Contra program
description: >-
  Look up a Contra program, discover the filters it supports, then list and
  filter the experts in that program by availability, rate, location, and
  skills using the Contra Public API.
api: openapi/contra-openapi-original.json
operations:
  - getProgram
  - listProgramFilters
  - listProgramExperts
---

# Find and filter experts in a Contra program

Use the read-only Contra Public API to browse the experts inside a Contra
program (identified by its `programNid`).

## Authentication

All calls require a Contra API key sent in the `X-API-Key` header (the
official `@contra/webflow` SDK also mirrors it into `Authorization`). A
missing or invalid key returns `401 {"code":"Unauthorized","message":"Missing
or malformed API key"}`.

Base URL: `https://contra.com` (endpoints live under `/public-api/`).

## Steps

1. **Get the program** — `getProgram`:
   `GET /public-api/programs/{programNid}`.
   Returns the `ProgramSummary` (title, subheader, expertsCount, totalHires,
   hireUrl, applyUrl). Use it to confirm the program exists and to show
   headline stats.

2. **Discover available filters** — `listProgramFilters`:
   `GET /public-api/programs/{programNid}/filters`.
   Returns the filter definitions the program supports, so you know which
   query parameters are meaningful before listing experts.

3. **List and filter experts** — `listProgramExperts`:
   `GET /public-api/programs/{programNid}/experts`.
   Supported query parameters: `available`, `languages`, `location`,
   `minRate`, `maxRate`, `sortBy`, and `limit` / `offset` for paging.
   Returns an `ExpertProfileList`; each `ExpertProfile` includes `name`,
   `oneLiner`, `hourlyRateUSD`, `averageReviewScore`, `skillTags`,
   `profileUrl`, `inquiryUrl`, plus embedded `projects` and `reviews`.

## Conventions

- **Pagination:** page experts with `limit` + `offset`
  (see `conventions/contra-conventions.yml`).
- **Errors:** `{code, message}` JSON envelope
  (see `errors/contra-error-codes.yml`).
- **Read-only:** every operation is a `GET`; there are no write or
  idempotency semantics.
