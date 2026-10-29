## Problem

The shared date parser reads month-first for tenants who write dates day-first. This is the original cause associated with desk ticket #4471, not the booking-v3 hint defect.

**Report count: 379 unique reports** in #4471, across 11 tenants, received 2026-04-01 through 2026-10-04.

## Evidence and reproduction

- R-41200: Harbour Dental, app, tenant locale en-GB, booking-v2. A 7 April reminder went out on 4 July.
- R-41201: same tenant/channel, reversed month/day example (4 July became 7 April).
- For a day-first tenant, pass 07/04/2026 through the shared parser. Expected: 7 April; faulty month-first interpretation: 4 July.
- A booking-v3 tenant's app/email reports still belong here. Only its form submissions belong to the separate hint issue.

## Count method and uncertainty

Run `cd support-kb && npm run analyze`. The unmodified `uniqueById`, `causeOf`, `dayFirst` and `groupByCause` functions deduplicate export IDs, filter mergedInto=4471, and classify using tenant locale/form plus channel. The export has 427 rows, but 412 unique reports. See `analysis.json` for the full partition of IDs.

Intake tags disagree: 356 were tagged parser and 23 hint. Tags/browser locale are not authoritative.

Five included reports (R-41202, R-41209, R-41236, R-41344, R-41620) read as a **one-day move**, not a demonstrated month/day swap. The export's desk notes and tenant-based classifier keep them in parser; reproduce separately before asserting a mechanism. Do not subtract them or open a third root-cause ticket without evidence.

## Fix and acceptance

- Parse using an explicit tenant-aware date format, not browser guesses or a universal month-first parser.
- Test app/email and booking-v2; ambiguous dates, day >12, validation and regression cases.
- Keep booking-v3 hint work separate; parser work alone cannot fix its misleading hint.
- Recheck new desk arrivals and recompute counts with a fresh export; this count is an export snapshot.

KB source: `support-kb/src/routes/kb/incidents/4471-reassessment/+page.svx`.
Rendered desk page: `support-kb/build/kb/incidents/4471-reassessment/index.html`.
