- Using only the BSB sources at `/run/media/deb/Bible/BibleHub_Files/`, I want to create a BSB Bible Study vault.
- Data assumption: sources live at `/run/media/deb/Bible/BibleHub_Files/` (external mount, not vault content).

## The Berean Standard Bible Text

- The BSB texts located at `Bible/` at vault root, e.g. `Bible/BSB/OT/` or `Bible/BSB/NT/`
- The texts are to be in canonical order
  - 01_Genesis
  - 02_Exodus
  - ...
  - 66_Revelation
- There should be only one verse per file
  - NN\_\_Book/Book_Chapter/Book_Chapter_Verse.md (double underscore to disambiguate `1_John` vs `John`)
    - 01\_\_Genesis/Genesis_01/Genesis_01_01.md
  - Chapter/verse pad 2-digit, except Psalms 3-digit (e.g. `Psalms_119_176`)
  - Chapter index file per chapter with static links, e.g. `Genesis_01.md`
  - Tagging is Hierarchical
    - #BSB/OT/01\_\_Genesis/Genesis_01/Genesis_01_01
  - There should be `previous` and `next` navigation between each verse. Pilot edges are null (no broken links outside pilot).

## Word Studies

- The BSB Strong's verse mapping should be located within the verse file
- These mappings are to be wikilinks linked to `Bible/Word_Studies/Hebrew/H0001 - translit` or `Bible/Word_Studies/Greek/G0001 - translit` (Option A: 4-digit pad, `-` separator, ASCII-folded translit, number prefix keeps collisions unique e.g. `H0001 - ab` vs `H0002 - ab`)
- Interlinear table from TSV (`hebrew|greek` + `bsb` gloss columns); link every Strong's including H853 `eth` gloss `-`, one link per Strong's per row
- These word study files list pilot verses only for pilot (full canon list at full build). Definition source is `bsb-interlinear lexicon/hebrew.json|greek.json` (`word/translit/pron/def/fullDef/kjv`)
- Tagging is Hierarchical
  - `#Word_Study/Hebrew/H0001 - translit` or `#Word_Study/Greek/G0001 - translit`

## Search Function

- Need to be able to search for terms within all verse files
- Need to be able to search by Strongs numbers
- Pilot uses core search + Omnisearch (installed). No Dataview queries inside verse/stub files. Static links only to avoid Dataview slowdown at full 31k + 14k scale.

## Pilot Scope (locked 2026-10-15, full canon built 2026-10-16)

- Pilot was Genesis 01 + 1 John 1 only. Full canon now built: 31,102 verses + 1,189 chapter indexes + 13,204 stubs = 45,495 files via `scripts/build_canon.py` (Aramaic verses carry text with deferred word-data note).
- Minimal verse frontmatter: `type/ref/book/chapter/verse/strongs/tags/prev/next` plus body quote + interlinear table + Strong's links + nav line.

## Study Layer

- Studies at `Studies/` vault root, e.g. `Studies/43__John/John_05_01-15 - Study.md`
- Built by `scripts/render_study_vault.py` (stdlib only, `--dry-run/--limit/--force`, idempotent, refuses overwrite without `--force`)
- Hybrid C: prefill mechanical only (embeds, textual notes, 4c word table, xrefs), interpretation blank
- Trim: Sec 1 embeds labeled plus quote, Sec 2 Textual Notes only, 3b points to 4c, 3e dropped (parsing stays in verse files), 4c word table kept
- Study tags `Study/NN__Book/Range` only, genre stays as frontmatter field. Aliases allowed in Studies only when tables do not break
- Genre auto map covers 66 books to 10 modules in IBS_Templates (gospel, epistle, ot_narrative, poetry, prophets, apocalyptic, law, wisdom, acts). Parable via `--genre` override. Genesis default is law. Psalms 3-digit chapters handled
- Entities and Doctrines created as studies generate, no bulk build
- Proofs grew into 128 studies (2026-10-21, see \_Memory/2026-10-20-handoff.md): Genesis 1-7, Exodus 20-21, Esther 1-7, Psalms 1-2, Proverbs 1-7, Micah 5-6, Luke 15-16, John 5-11, Acts 1-7, 1 John 1-2, Revelation 1-2. All heading aligned with headings in frontmatter, title, and Sec 1
- Doctrines Index rows sorted doctrine A-Z then canonical book. Findings columns left blank for study work


