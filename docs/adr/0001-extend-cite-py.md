# Extend cite.py rather than adopt an existing tool

citation-check grows out of `cite.py`, the script written for the Places report (feup.devops.places PR 17), and moves to its own repo, `Alef-Keuffer/citation-check`, symlinked into `~/.claude/skills/citation-check`. The user's condition was to build only if nothing like it already existed. A survey on 2026-11-14 found tools that do parts of the job, but none that reads pandoc citations or resolves chapter and section locators.

## Considered options

| Tool | Does | Missing for these reports |
|---|---|---|
| citefact (github.com/hearthresearch/citefact, MIT, v0.3) | Deterministic check that each citation is in the bibliography and each quote is verbatim; `p.`/`pp.` checked against printed page numbers from running headers; optional AI judging of claims | Reads only author-year text such as "(Author, year, p. n)", not `[@key]`; no `ch.`/`sec.` locators; its README says outright it doesn't check bibliography details; quotes must be in the main text; needs `uv` and Docling |
| citations (github.com/elliottower/reproducible-science) | Quotes in YAML files tied to a hash of each source; exact match after folding case, ligatures and hyphenation; checks pages | Records sections but doesn't check them; checks the bibliography against DOI records, not the source; built for LaTeX |
| Starbuck (github.com/tiagojct/starbuck) | Reads pandoc `[@key]`; checks entries against public catalogues | Doesn't check the author's quotes |
| verbatim-quote, LLM-based skills (mronkko/claude-academic-research, edwinhu/workflows), an Obsidian plugin | Partial | A matching library only, or not deterministic |

- **Adopt citefact.** Rejected. Nearly every citation in the Places report uses `ch.`/`sec.` or no locator, and citefact checks neither. Its README rules it out without a trial, so installing `uv` and Docling to try it is not needed.
- **Extend cite.py (chosen).** It already does what none of the others do: reads pandoc `[@key]`, finds the local PDF by the ISBN it prints, and resolves `ch.`/`sec.` through the PDF's bookmarks.

## Consequences

- Two ideas are borrowed: the normalisation rules of `citations` (which cite.py mostly follows already) and citefact's split between "passage not on that page" and "passage not in the source".
- Teammates get the tool by cloning its repo; a report repo's `AGENTS.md` points to it.
- Input is pandoc Markdown with a BibTeX file only; verify finds the report through the one pandoc defaults file that names both `bibliography` and `input-files`.
