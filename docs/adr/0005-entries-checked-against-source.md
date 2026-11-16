# Entries are checked against the source, with a one-year catalogue margin

Each field of an entry (author surnames, title, edition, publisher, year) must appear in the source itself: the PDF's first pages, or the saved web page. The year is the copyright year. Verify also compares it with the catalogue's year for the ISBN: a gap of one year passes, with both years shown, and a larger gap fails.

## Considered options

- **Leave the entry's fields unchecked.** Rejected by the user: the entries are typed by hand or by an AI, so they need a deterministic check too.
- **Generate entries from catalogues.** Rejected: on 2026-11-14 doi.org returned BibTeX for RFC 9457 and Open Library returned all three Places ISBNs, but the catalogues disagree with the books. Open Library gives Newman 2020 where the book says 2021, and adds a series name to Wiegers' title.
- **Check against the source (chosen).** The PDF you quote from is the one the citation describes. It works offline, the same way the ISBN match does. It found a real error at once: the Places entry `richardson2018` says 2018, but the PDF prints only "©2019".
- **Year must equal the catalogue's.** Rejected: two of the three Places books would fail for no real error.

  | Book | Copyright page | Open Library |
  |---|---|---|
  | Wiegers & Beatty | 2013 | 2013 |
  | Newman | 2021 | 2020 |
  | Richardson | 2019 | Nov 2018 |

  A book released late in the year often carries the next year's copyright. A gap of more than one year usually means the wrong edition; Newman's 1st and 2nd editions are six years apart.

## Consequences

- An entry's key should match its year, so the Places report renames `richardson2018` to `richardson2019`.
