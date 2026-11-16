# A citation reaches a level, and only Verified passes

Verify gives each citation one level: Unknown, Exists, Located, Found remotely or Verified (see GLOSSARY.md). Anything below Verified fails, in a single mode with no separate draft mode.

## Considered options

- **Two outcomes, checked or unchecked** (the first SKILL.md). Rejected: the user often starts from citations an AI generated, for sources not at hand, and wants to know how far each one got.
- **Five levels (chosen).**
  - *Exists* comes from a catalogue: Open Library by ISBN, doi.org by DOI. Both answered for the Places entries on 2026-11-14. It proves the work exists, not that it says the claim, and catches invented references while drafting.
  - *Located* comes from a table of contents that acquire saved from the publisher's page. Open Library has none for the three Places books, so publisher pages are the realistic source. The judge sees the chapter title next to the claim, which catches "ch. 5 is about something else".
  - *Found remotely* comes from the Google Books API, and is kept only if a trial passes: real phrases from the three Places books found, an invented one not. Otherwise it is dropped.
  - *Verified* is the full check against the source.
- **Digests**, judging a claim against a published summary of the chapter. Out of scope for now: it is an AI reading secondhand text, with nothing deterministic underneath.
- **A draft mode that only warns.** Rejected: failing while drafting is expected, the failures are the to-do list, and a second mode is a second set of rules to keep in step.

## Consequences

- For each failure, the fix is to get the source (a PDF in `docs/sources/` or a URL for acquire), swap in a source at hand, or cut the claim.
