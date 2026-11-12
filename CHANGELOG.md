# Changelog

All notable changes to the TR.aai-bguard-cpsec source are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Dates are ISO 8601 (YYYY-MM-DD).

## [Unreleased]

### Added

- Full reStructuredText source for Technical Report TR.aai-bguard-cpsec,
  converted from the ITU-T-format Word contribution
  `FGTIDA-TR.aai-bguard-cpsec.docx` (13 clauses, 4 appendices, bibliography,
  7 tables, 5 figures, no tracked changes or comments).

### Changed

- **Structure.** 20 top-level parts in one flat level: clauses 1–14 numbered,
  Appendix I–IV, Bibliography and Annex A unnumbered.
- **Conventions inserted as clause 5** (body `None.`) per ITU-T Technical Report
  house style. Source clauses 5–13 became 6–14, and every in-text reference to a
  clause at or after the insertion point was shifted by one — including the
  bare subsection lists written as `Assessment factors: 10.2, 10.5, …`, which
  now read `11.2, 11.5, …`, and the ranges `10.2 to 10.11` → `11.2 to 11.11`,
  `12.1 to 12.9` → `13.1 to 13.9`. Table and figure numbers are unaffected:
  they are sequential across the whole document, not per clause.
- **Annex A moved.** The A.1 justification form arrives in the Word file's front
  matter, between "Background and Proposal" and "Summary". It is rendered as an
  unnumbered annex after the Bibliography, as a `Field | Content` two-column
  table. The Word form is a 5-column grid whose first row carries four different
  facts (working group, work item type, meeting venue and date); those are
  decomposed into separate labelled rows rather than concatenated.
- **Front matter** moved from the Word cover into `index.rst`: Abstract (from the
  cover field), Background and proposal, Summary, History, Authors, Keywords,
  Foreword and Note. `conf.py` carries `project`, `release`, `author`,
  `copyright` and the four `cover_*` values; the History table is left empty
  pending approval records.
- **Numbering.** `numfig = False`; captions keep their document-wide numbers
  (Table 1–7, Figure 1–5) because the prose cites them by number — for example
  "Table 7 maps each standardization point of clause 13". Sphinx's `numfig`
  numbers per section and restarts inside unnumbered appendices, which would
  break those citations.
- **Abbreviations** rendered as a two-column table (44 entries, `AgBOM`–`Wasm`).
- **Definitions** in 3.1 rendered as a definition list, each entry carrying its
  source citation (5 entries).
- **Bibliography** rendered as definition lists in three groups (Standards;
  Regulatory and policy; Industry and civil-society sources), 22 entries.
- **Template example content removed**: the demonstration chapters and the
  OpenAPI sample (`src/doc/yaml/example.yaml`) and its `html_extra_path` entry.
  Archived under `local/archive/`, which is not tracked.
- **PDF cover** now reads "Technical Report" rather than "Recommendation".

### Fixed

- `exclude_patterns` extended to `**/_*.rst` so fragments in subdirectories are
  not built as standalone pages.
- **Word highlight spans in headings.** Seven headings arrived wrapped in
  `<span class="mark">` (5.2, 5.3, 8.3, 8.4, 10.1, 11.3 and 13); stripped.
- **Heading level error.** 10.2–10.11 arrive one level deeper than 10.1 and
  10.12 in the Word file; flattened so the assessment factors number as
  siblings — `11.2 Prevention factors (AF-01)` … `11.11 Cross-cutting
  observation factors (AF-10)`, with `11.12 Collection methods`. Without this
  the factors rendered as `11.1.1`–`11.1.10` and every
  `Assessment factors: 11.x` reference pointed past its target.
- A bold span closed mid-word in clause 9 (`safe-state transition**s`), which
  made docutils emit "Inline strong start-string without end-string".
