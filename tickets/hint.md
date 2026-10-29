## Problem

Booking-v3 prints `MM/DD/YYYY`, but interprets form input using the tenant's day-first locale. A person following that hint enters a date that the form reads in the opposite order. This is separate from the shared parser defect in #4471.

**Report count: 33 unique reports** merged into #4471, across three tenants, received 2026-04-17 through 2026-09-22.

## Evidence and reproduction

- Affected: Alder Physio (de-DE), Copper Studio (es-ES), Pale Harbour (pt-PT), all booking-v3.
- R-41241: Alder Physio, form; two people arrived on 8 June but were booked for 6 August. Browser en-US and the tenant de-DE are not interchangeable evidence.
- On one of these tenant forms, follow the printed MM/DD/YYYY hint and enter 08/06/2026 intending 6 August. The tenant's DD/MM reader yields 8 June instead.
- App/email for these same tenants belongs to the shared-parser issue, not this hint issue.

## Count method

Run `cd support-kb && npm run analyze`. The export's actual classifier resolves each report's tenant locale and form, checks day-first with Intl, and then checks the channel. Deduplicate IDs before counting; see `analysis.json` for every ID.

The intake labels are misleading: 18 were tagged hint, 15 parser. Tenant configuration plus channel, not intakeTag or subject text, establishes this partition.

## Fix and acceptance

- Generate the displayed date hint and its interpretation from the same explicit locale-aware format.
- Test form submissions for all three affected tenants and ensure app/email still route to shared-parser investigation.
- Include ambiguous and unambiguous dates and validation errors.
- Stop merging this cause into the shared-parser ticket. Matching subjects do not establish a shared cause.
- Recompute counts on future exports while the desk keeps receiving reports.

KB source: `support-kb/src/routes/kb/incidents/4471-reassessment/+page.svx`.
Rendered desk page: `support-kb/build/kb/incidents/4471-reassessment/index.html`.
