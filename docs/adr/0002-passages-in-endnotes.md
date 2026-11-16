# Every citation carries its passages in an endnote

Every citation is followed by a footnote holding one or more verbatim passages, each with its location: `[@newman2021, ch. 3]^["passage" (PDF p. 104)]`, or `(p. 71)` when the book prints page numbers. LaTeX's `enotez` turns the footnotes into endnotes, and `\printendnotes` prints them in a quotations appendix, so the main text gains only a superscript number. The one exception is a pointer citation, `[see @key]`, which carries no passage.

## Considered options

- **Where the passages live.**
  - *Not stored, found again by the agent on each check* (how the first SKILL.md worked). Rejected: every check costs an AI run, and nothing is left for a reader or a script to re-check.
  - *A footnote at the bottom of each page.* Rejected by the user: it clutters the page.
  - *A hidden comment, `<!-- quote: … -->`.* Rejected: the PDF's reader never sees the evidence.
  - *A Lua filter that builds the appendix from hidden attributes.* Rejected: about 30 lines of filter code for what `enotez` does with a preamble line.
  - *Endnotes in an appendix (chosen).* Stored passages let verify re-check every one without an AI; a passage moves with its citation when the text is edited; the reader sees claim and evidence side by side, so judging an overstated claim takes one look. Tested with pandoc and xelatex on 2026-11-14: the text shows a superscript, the appendix lists the quote. Not yet checked: whether the superscript links to the appendix.
- **Which citations need a passage.**
  - *Only those with a locator.* Rejected: `[@postgresql]` has no locator because its entry already names one section, yet it backs a specific claim, so the rule would let it through.
  - *All, except those the author marks `see` (chosen).* "See" is the usual academic signal for a pointer, not evidence; readers see it, the script needs no judgment, and verify counts the pointer citations so overuse shows. The agent never writes `see` itself: when no passage carries a claim, it rewords the claim, cuts it, or asks.
- **The location of a passage.**
  - `p.` always means the printed page, read from the running header or footer; roman-numbered front matter counts. A source that prints no page numbers (Newman's PDF) fails a `p.` locator with "no printed page numbers; cite ch. or sec.".
  - For such a source, the main text keeps the chapter or section and the endnote names the PDF page: the user wanted an exact spot even without printed pages.
  - *Paragraph numbers.* Rejected: a script can count paragraphs reliably on web pages but not in PDFs, whose text extraction loses paragraph breaks; the passage already pins the spot, and paragraph counts on live documentation shift with each edit.

## Consequences

- A citation with several keys, `[@a; @b]`, is split so that each key has its own footnote.
- A claim with two parts gets two passages in the same footnote.
- All of a report's footnotes become endnotes, so a report that wants ordinary footnotes as well needs a second note type.
