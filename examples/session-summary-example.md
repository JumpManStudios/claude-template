<!--
  A worked example, not a template. This records real work in a public repository
  (JumpManStudios/fact-bank-resume-builder, commit 7ed5666), so every path, decision, and
  number below can be checked against the actual commit.

  Copy the shape, not the content. The template to fill in is
  templates/session-summary-template.md.
-->

# Session Summary: Header-aware chunking for the resume-curator index

**Date:** 2026-08-13
**Scope:** Make the MCP server's search return the relevant *section* of a long markdown file
instead of the whole file.

## What I did

Replaced whole-file indexing in the `resume-curator` MCP server with section-level chunking.

`chunkMarkdown()` in `mcp/resume-curator/src/indexer.ts` now splits a markdown file on `H1`/`H2`
boundaries and emits one chunk per section, each indexed as its own TF-IDF document. `H3` and
deeper stay inside their parent `H2` rather than becoming boundaries of their own.

Each chunk carries an id of `path#slug` (from a `slugify()` helper over the heading text), the
source `path`, the `heading` text, and the section body. Content appearing before the first
heading becomes a preamble chunk keyed by bare `path`.

Threaded the chunk type through the two consumers: `src/index.ts` at index build, and
`src/tools.ts` so search results carry the heading and can return a section-scoped snippet.

Verified against the existing self-test rather than adding a new harness:
`npm run selftest -- "graphql"`.

## Decisions

**Chunk on `H1`/`H2` only, not every heading level.** Splitting on `H3`+ fragments a document
into pieces too small to carry meaning on their own — a three-line subsection matches on a
keyword and returns without the context that makes it useful. `H2` is where a document changes
subject. Rejected splitting on all levels, and rejected a fixed token-window chunker: the latter
is what you reach for when documents have no structure, and these are hand-written markdown with
reliable headings. Using the structure that's already there beats imposing one.

**Files with no `H1`/`H2` become a single whole-file chunk, keyed by bare `path`.** This is the
backward-compatibility path: unstructured and heading-less files index exactly as they did
before, so the change is strictly additive. Rejected requiring headings — that would have made
indexing fail on legitimate input, converting a search-quality improvement into a hard error.

**Chunk ids are `path#slug`, derived from heading text.** Human-readable and stable under
document reordering, which a positional index (`path#3`) would not be. The cost is that renaming
a heading changes the id, so anything holding a reference to it goes stale — accepted because
these ids are not persisted anywhere outside a session-lifetime index. **This is the decision to
revisit first if ids ever start being stored.**

**`slugify()` joins alphanumeric runs rather than stripping unwanted characters.**
`match(/[a-z0-9]+/g).join("-")` cannot backtrack. The strip-and-collapse form of this function is
the classic shape for a catastrophic-backtracking regex, and this runs over arbitrary
user-authored headings.

## Files changed

- `mcp/resume-curator/src/indexer.ts` — modified; added `chunkMarkdown()`, the `Chunk` interface,
  and `slugify()`. The substance of the change (+69/−2).
- `mcp/resume-curator/src/tools.ts` — modified; results carry heading and section-scoped snippets
  (+10/−2).
- `mcp/resume-curator/src/index.ts` — modified; build the index over chunks rather than files
  (+4/−1).

Three files, +76/−7 total, in commit `7ed5666`.

## Impact

Search over a long markdown file now returns the section that matched instead of the entire file,
which is what makes `find_evidence` usable — given a resume bullet, it can surface the specific
passage backing it rather than a document to read through.

Heading-less files are unaffected by design, so no existing behavior regressed.

**Not measured.** No before/after retrieval-quality benchmark was run — the improvement is
structural and was confirmed by inspecting the self-test's snippets, not quantified. Building a
real relevance comparison would need a labelled query set, which doesn't exist yet.

## Next steps

- Run the indexer over a full `source/` tree rather than the skeleton's near-empty one. Chunk
  count and section sizes have only been seen on small input.
- Decide whether `find_evidence` should return more than one chunk per file. It currently returns
  the best-matching section, which is wrong when a claim is supported by two separate passages.

Deliberately not done: no token-window fallback for very long sections. Worth adding only if real
content produces sections too large to be useful as single chunks — speculative until then.

## Open questions

- Is `H2` the right boundary for every file type being indexed, or only for session summaries and
  the fact bank? A document that uses `H2` merely decoratively would chunk badly, and none has
  been checked for this.
- Unverified assumption: heading text is unique within a file. Two identically-named `H2`s produce
  colliding chunk ids, and nothing currently detects or disambiguates that.
