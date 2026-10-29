# MEGA S01E03 — what it shows

Reassessment of support ticket #4471 using the supplied KB export dated 2026-10-04.

| Cause | Unique reports | GitHub issue |
|---|---:|---|
| Shared month-first parser for day-first tenants | 379 | [#1](https://github.com/kacperborowieckb/mega-s01e03-what-it-shows/issues/1) |
| Booking-v3 hint conflicts with tenant date order | 33 | [#2](https://github.com/kacperborowieckb/mega-s01e03-what-it-shows/issues/2) |

427 export rows → 412 unique IDs. Five one-day-move reports remain in the tenant-derived parser bucket with an explicit uncertainty note.

## Final engineering disposition

The course's later desk update closes #4471 on 5 October: parser **fixed** (`fix(dates): parse dd/mm for eu locales`); hint **won't fix** because booking-v3 was retired. GitHub #1 is completed; #2 is not planned. Retirement is not a hint repair.

## Deliverables

- [Signed KB source](support-kb/src/routes/kb/incidents/4471-reassessment/+page.svx)
- [Rendered desk page](support-kb/build/kb/incidents/4471-reassessment/index.html)
- [Replacement decision proposal](support-kb/src/routes/kb/decisions/0002-dedupe-by-cause/+page.svx)
- [Reproducible partition and report IDs](analysis.json)
- Original exported files are retained unchanged. New pages are indexed automatically by KbIndex.

## Reproduce

Requires Node 22.18+ (native TypeScript support) and npm.

```sh
cd support-kb
npm ci
npm run analyze
npm run build
npm run preview -- --port 4173
```

Open http://localhost:4173/kb/incidents/4471-reassessment/ to inspect the generated table, queue timeline, dedupe explorer and desk ref **DR-V0QR-SWZZ**. The build directory contains the entire static KB and its assets; serve it as a directory rather than opening a lone HTML file.

The page counts are computed by the original Svelte components, not handwritten into its prose. Accepted decision 0001 is not edited. Decision 0002 is a proposal awaiting the support owner's acceptance.

## Verification

Analysis asserts the exact partition and total. Production prerender succeeds. Browser inspection confirms 379/33, five one-day reports, the differing intake tags, timeline, dedupe examples and a real signature stamp.

`npm audit` reports six low-severity development dependency advisories. No forced upgrades were applied. This is a local build and static output, not a deployed SvelteKit server; review dependencies again before using it for a production service.
