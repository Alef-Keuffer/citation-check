# citation-check

A tool and Claude Code skill that checks the citations in a pandoc Markdown report against the cited sources themselves: each claim backed by a verbatim passage, found at the place the citation names.

## Language

**Citation**:
A pandoc `[@key]` or `[@key, locator]` in the report's text, with the footnote that carries its passages.
_Avoid_: Reference (when the in-text mark is meant)

**Entry**:
A record in the report's BibTeX file, named by its key.
_Avoid_: Reference, citation

**Claim**:
The sentence of the report that a citation backs, up to the citation.
_Avoid_: Statement, assertion

**Source**:
What an entry is checked against: a PDF in `docs/sources/` matched by the ISBN or DOI it prints (or by the entry's `file` field), or a saved copy of the entry's web page.
_Avoid_: Book (when papers and web pages are included), document

**Locator**:
The part of a citation that says where in the source: `ch.`, `chs.`, `sec.`, `secs.`, `p.` or `pp.`.
_Avoid_: Pinpoint, reference

**Passage**:
A verbatim stretch of the source, quoted in the citation's footnote with its location, `(p. 71)` or `(PDF p. 104)`; web sources carry no location.
_Avoid_: Excerpt, snippet (a snippet is what Google Books returns)

**Printed page**:
The page number the book prints in its running header or footer; what `p.` means.
_Avoid_: Page (when it could be a PDF page)

**PDF page**:
The position of a page in the PDF file, counted from 1; written `PDF p.` and used only when the source prints no page numbers.
_Avoid_: Page

**Pointer citation**:
A citation that starts with `see`; it points the reader to a work and carries no passage. Only the author writes one, never the agent.
_Avoid_: Whole-work citation, reference

**Acquire**:
The step that fetches sources and saves them, with their URL and date, into `docs/sources/`; off unless switched on for a run, and only from openly available sources.
_Avoid_: Download, fetch (as the step's name)

**Verify**:
The deterministic, offline step that checks every citation and entry against the saved sources only.
_Avoid_: Check (as the step's name), validate

**Judge**:
The step that decides whether a citation's passages carry its claim, first with a decision model and, below the threshold, with an Opus subagent.
_Avoid_: Review, grade

**Level**:
How far a citation got in verify: Unknown, Exists, Located, Found remotely or Verified. Only Verified passes.
_Avoid_: Status, tier

**Unknown**:
No catalogue knows the entry's ISBN or DOI; the work is probably invented.

**Exists**:
A catalogue (Open Library, doi.org) knows the work and its title and authors match the entry, but the source is not at hand.

**Located**:
The source is not at hand, but its saved table of contents has the locator's chapter or section, whose title verify prints.

**Found remotely**:
Google Books found the passage in the work by ISBN; kept only if its trial passes.

**Verified**:
Every passage is verbatim in the source, inside the locator's range and on the page it names, and the entry's fields appear in the source.
_Avoid_: Checked, valid

**Decision model**:
A model that answers a yes/no question with a probability instead of text, or returns a native score, such as Jev or MiniCheck.
_Avoid_: Classifier, small model

**Support score**:
A decision model's probability of "yes" or its native score, on a 0–1 scale.
_Avoid_: Confidence, rating

**Threshold**:
The support score at or above which judge accepts a citation without escalation; set per model by testing on real and overstated claims.

**Escalation**:
Sending a citation below the threshold to an Opus subagent, which reads the whole cited section and proposes a fix.
_Avoid_: Retry, second opinion
