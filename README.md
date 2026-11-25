# Repo Listing Auditor

Paste a list of repository names and see how the collection reads at listing depth: which first hyphen tokens are concentrated, and which sorted neighbours collide as one unreadable block. A slug echo check runs alongside and finds nothing here.

**Live demo:** https://0xelitesystem.github.io/repo-listing-auditor/

## Use

The full statement of what this does: paste a list of repository names and see how the collection reads at listing depth: which first hyphen tokens are concentrated, and which sorted neighbours share so many leading characters that they render as one unreadable block. A slug echo check runs alongside and currently finds nothing on the author's own 373 repos.

1. Open the page and paste your names into the box. Four input shapes are accepted, sniffed in this order: a JSON array of objects (first key matching name, slug, repo or id becomes the name; description, desc or d becomes the description), tab separated name and description, comma separated name and description with double quoted fields, or a bare newline separated list of names. A bare list with no descriptions at all is enough.
2. Pick a collation. Codepoint order (ASCII) is the default. Punctuation insensitive collation is offered as a labelled alternative.
3. Set the adjacent collision threshold. It defaults to 8 shared leading characters, and the panel heading follows whatever you set.
4. Read the panels. A is prefix concentration. B is the runs and the adjacent pairs. C is a mock listing so the blocks are visible rather than merely counted. D is the secondary description checks.
5. Press **Load the 373 row example** to fill the box with the reference corpus every figure below was computed from, so you can reproduce them here in one click. Press **Load the 40 row detector sample** for neutral placeholder data that exercises every panel, which the reference corpus does not. Press **Run self test** to make the page prove its own primitives, and its own published figures, in front of you.

Duplicate names are removed and counted, on the full name rather than a shortened copy of it. Rows with an empty name are skipped and counted. Malformed input produces a named refusal and no result at all, never a partial one: an unterminated quoted field, a quote sitting part way through an unquoted field, a closing quote followed by anything but a comma or a line break, and a document whose lines disagree about whether the delimiter is a tab or a comma are all refused by name, with the line number, rather than split on a guess.

## Why this exists

A catalog of a few hundred repositories does not fail one repo at a time. It fails at listing depth, where forty names scroll past in the same second and nine of them start with the same six characters. That failure is invisible in any per repo check, because every one of those repos is individually fine.

There is a second reason. A hand written map of the same catalog recorded its prefix runs as "lexicographic" without saying which collation that meant, and got several of them wrong as a result: a block of nine recorded as four plus five, another block of nine recorded as seven, an adjacent pair at ten recorded as nine. Computing it beats remembering it, and exposing the collation choice beats assuming one. That is why the toggle is on the page instead of a silent default.

There is also a reason it is shaped the way it is. A repository list is not something you should have to upload anywhere to have counted. So this is one file with no network call, no telemetry, no account and no browser storage, MIT licensed, and it works with the network cable pulled out.

## Accuracy

Everything below was computed at build time by running the shipped functions in this repository's `index.html` over a 373 row corpus of real repository names, not copied from any planning document. The corpus is the author's own public repository list, and it ships inside the page in full, so the **Load the 373 row example** button reproduces every figure below in one click rather than asking you to take it on trust. It is shipped untrimmed and unencoded: no row is dropped, reordered, shortened, reworded or substituted. Two third party product names appear in seven of these repository names and in twenty four of the descriptions, 34 occurrences in all. Those are the author's own repository names, and they are stored literally, the same way every other vendor name in the corpus is. The self test asserts the text is 73559 characters over 373 rows and checks its first and last rows against the snapshot.

The corpus is a snapshot, not a feed. It is a 373 row capture taken on 2026-09-02, and every figure below is stated for that snapshot and for no other. The live list is public at https://github.com/0xelitesystem and it moves: repositories have been added since, so a list pulled today is not the same 373 rows and will not give the same totals. To run the method on your own list instead, paste it in any shape the tool takes: one name per line, optionally a tab and then the description.

Under the default codepoint order, over those 373 names:

- 28 first hyphen tokens appear 3 or more times, led by `ai` at 10, then `llm`, `json` and `agent` at 9 each, `prompt` and `mcp` at 8, one vendor named token at 7, and `api` at 6.
- 28 maximal contiguous runs of 3 or more.
- 50 adjacent pairs share 8 or more leading characters. The worst is `affiliate-disclosure-reference` against `affiliate-disclosure-templates` at 21 shared characters, then two pairs at 17.
- The `agent` block is ONE maximal run of 9, `agent-blast-radius` through `agent-trace-viewer`. The name `agentic-workflow-patterns` lands at sorted index 16 against `agent-trace-viewer` at index 15, because the hyphen at codepoint 0x2D precedes the letter i. It splits nothing. The hand written map that said four plus five was computed under a different collation while calling itself lexicographic.
- The `llm` block is ONE maximal run of 9, `llm-eval-datasets-starter` through `llm-vram-calculator`.
- `lcp(embedding-cost-estimator, embedding-similarity-calibrator)` is 10, and the next pair in that group is 9. It is not nine and nine.

Under the alternative punctuation insensitive collation, over the same 373 names: 29 runs of 3 or more instead of 28; still 50 adjacent pairs at 8 or more, but not the same 50 (four pairs enter and four leave); the `agent` block splits into 4 then 5 with `agentic-workflow-patterns` between them; the `llm` run is 7; and the two embedding pairs become 9 and 9. Both orders were computed with the same shipped code. Neither is presented as the truth about any particular listing page, because no document on hand states which collation such a page uses.

Description checks over the same 373 rows: 0 slug echoes by exact token multiset equality, 0 by exact title cased slug string match, 0 empty descriptions, and exactly 1 row above token set Jaccard 0.45, at 0.667, which is a product name legitimately repeating its own words and not a defect. The next highest Jaccard in the whole corpus is 0.267.

Historical: a 2026-08 audit of a then 346 repo catalog passed 193 repos whose description was the title cased slug, because the check asserted not empty instead of testing the property. Current: recomputed over the 373 live rows on 2026-09-02, the count is 0.

The WARN tier is one narrow anchored regular expression, and it prints its own failure rate on the page: 1 hit, 0 true positives, 1 false positive (`conventional-commits-reference`) on that 373 row corpus. It is labelled a string match rather than a grammatical judgement, because clause parsing cannot ship in one self contained page with no network and no library. It is never added to any headline total. A broader token based variant was written, measured at 40 hits on the same 373 rows with obvious false positives, and rejected; the page names two of them. The variant counts a description as a hit when any of `be, is, are, was, were, been, being, am` appears before the first of `that, which, because, when, if, while, so, although, though, unless, until, whether, where, as, since, than, before, after`. Those two word lists are stated here so the figure of 40 stays recomputable from this file alone.

A near duplicate name detector by edit distance was also written and measured. At distance 3 or less with a first token bucket prefilter it returns zero pairs on the same corpus, so it is not shipped in any form.

What is not verified: nothing here claims to know the collation used by any repository host's own listing page, and no ranking, traffic or discovery effect is claimed. This tool measures how a list of strings reads when sorted, and nothing else.

## Privacy

Everything runs in your browser. Nothing is uploaded, stored or transmitted. The page makes no network request of any kind: no fetch, no background request, no socket, no beacon, no analytics, no external font, no third party script. It writes nothing to browser storage. Open it from a local disk with no network connection and every feature still works, which is what makes that claim checkable rather than promised.

## Run locally

```
git clone https://github.com/0xelitesystem/repo-listing-auditor.git
cd repo-listing-auditor
```

Then open `index.html` in a browser. That is the whole procedure. If you prefer to serve it:

```
python -m http.server 8000
```

and open `http://localhost:8000/`.

## Build

No build. One self contained `index.html` with inline styles and inline script, no dependencies, no package manager, no bundler, no build step. The self test is inside the page, on a button.

## Accessibility

- Contrast verified by arithmetic on the token values in both themes, not by looking at a rendered page, because semantic colours do not paint until a finding exists. Worst pair is 5.06 to 1 in the light theme and 7.77 to 1 in the dark theme, both above the 4.5 to 1 minimum. Control boundaries clear the 3 to 1 non text minimum.
- `prefers-color-scheme` is honoured.
- No result is carried by colour alone. Every marked line in the mock listing carries a word tag, RUN, PAIR or RUN and PAIR, plus a written reason such as "run of 9 on the token agent, member 3" or "shares 21 leading characters with the line above". Both members of a collision are marked, one naming the line above and one the line below, so neither is ever printed as having no adjacent collision. The other panels tag with CLEAN, CONCENTRATED, COLLISIONS, ECHO, NEAR, WARN, COUNT, SKIPPED and UNDEFINED, and the self test with ALL PASS or FAIL. Every finding survives greyscale, colour vision deficiency and a screen reader.
- The collation control is a real radio group inside a fieldset with a legend, not two styled buttons, because it changes every number on the page.
- Every input has a programmatic label. Every table is a real table with a caption and `scope` on its header cells. The mock listing is an ordered list, since sorted position is the point.
- Results are announced through one polite live region that speaks the summary line only, never the individual rows, and recomputation is debounced so it does not fire mid typing.
- Every control is reachable and operable by keyboard, with a visible focus outline.

## Third-party notices

No third-party code is reproduced in this repository. No library, framework, font or stylesheet is vendored, bundled or loaded. Where behaviour follows a published specification, that behaviour was reimplemented from the specification and no code is reproduced here. The comma separated parser follows the field and quoting rules described in RFC 4180. The alternative collation uses the browser's own built in collator through the ECMAScript Internationalization API; nothing is bundled to provide it.

This project is independent and unaffiliated with any repository host, package registry, standards body or vendor named anywhere in it. No endorsement is implied by any name that appears here.

## Related

- https://github.com/0xelitesystem/title-to-url-slug
- https://github.com/0xelitesystem/markdown-heading-toc-linker
- https://github.com/0xelitesystem/json-formatter-and-validator

## License

MIT. Copyright (c) 2026 RBL Industries LLC. See [LICENSE](LICENSE).
