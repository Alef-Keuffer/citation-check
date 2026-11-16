# Acquire and verify are separate steps

Verify is deterministic and offline: it reads only the files saved in `docs/sources/` of the main checkout, so worktrees share them. Acquire is best-effort, may use the web and an AI, and is off unless switched on for a run (`/citation-check acquire=on`). It saves each thing it finds with its URL and date: the entry's web page, an RFC, an open-access copy found by DOI (Unpaywall, arXiv), a table of contents from the publisher's page, a Google Books snippet. It fetches only openly available sources; the user supplies the PDF of a paywalled book.

## Considered options

- **Verify fetches web sources itself** (the first cite.py: a `url` entry is fetched with curl on every run). Rejected: the result depends on the network (a run on 2026-11-14 failed once with an HTTP/2 framing error that a retry didn't reproduce) and on live documentation that can change under a quote.
- **Acquire always on.** Rejected: the user wants downloading to be a choice, and checks must be reproducible offline.
- **Anna's Archive as a source.** Rejected: it distributes copyrighted books without the publishers' permission, so the tool won't download from it.
- **Split, acquire off by default (chosen).** Acquisition can be messy and verification still repeatable: the same files always give the same result. A dated snapshot of a web page means a later edit can neither silently break a quote nor silently fix it.

## Consequences

- A paywalled book the user doesn't own stays below Verified (ADR-0004). The legal routes are the university library's e-book subscriptions, Internet Archive lending, or a PDF the user owns.
- No free service could confirm a phrase inside the three Places books without owning them (tested 2026-11-14 with real and invented phrases): Open Library search-inside has no scans of those editions, the Internet Archive scan of Wiegers 3rd ed. is lending-restricted, HathiTrust holds none of them and returns 403 to scripts, O'Reilly and Manning need a login and forbid automated search. The Google Books API is the one candidate; it needs the user's key (`GOOGLE_BOOKS_API_KEY`) and returns a snippet, not a page.
- Keys come from the environment only, never from the repo or the chat.
