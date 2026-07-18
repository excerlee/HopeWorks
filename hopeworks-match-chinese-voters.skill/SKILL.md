---
name: hopeworks-match-chinese-voters
description: >
  Match a voter file (or any name list) against a Chinese surname list to flag likely
  Chinese-ethnicity voters for canvassing, adding "Chinese (Y/N)" and "Country" columns.
  Use whenever the user has an uploaded voter/resident spreadsheet AND a Chinese surname
  list (e.g. one built by the canvassing-surname-lists skill) and asks to match, flag, tag,
  or identify Chinese voters/residents in that file. Trigger for "match last names to
  Chinese voters", "add a Chinese Y/N column to this voter file", "flag the Chinese voters
  in this list", or similar. Handles the common false-positive case where a matched surname
  (Lee, Lim, Chang, Yu, etc.) actually belongs to a Korean voter, by checking birth place
  and given-name patterns and labeling those rows "Korea" instead of counting them as
  Chinese matches.
---

# Match Chinese Voters

Takes an existing name list (typically a voter file) and a Chinese surname reference list, and
adds two columns — `Chinese (Y/N)` and `Country` — inserted immediately before the last-name
column so they're easy to eyeball next to the name they describe.

## Prerequisites

This skill needs two inputs:
1. **A surname reference list** — a workbook with a "Combined Master List" (or equivalently
   structured) sheet of `Romanized Name -> Country` pairs, one spelling per row, plain ASCII.
   If the user doesn't have one yet, run the `canvassing-surname-lists` skill first to build it.
2. **A name list to match against** — typically a voter file with at minimum a last-name column.
   Two more columns make the match dramatically better if present and should always be checked
   for and used: `birth_place` (self-reported country of birth — far more reliable than any
   surname guess) and first/middle name columns (for the Korean-name heuristic below).

## Workflow

### Step 1 — Load the surname reference

Build a lookup: `SURNAME (uppercase) -> {set of countries}` from the reference list's combined
sheet. One surname can map to multiple countries (e.g. "Wong" appears in both the Hong Kong and
Malaysia lists) — keep all of them, don't collapse to one.

### Step 2 — Identify the relevant columns in the target file

Locate (don't assume fixed positions — read the actual header row): last name, first name,
middle name (if present), and birth place (if present). If `birth_place` exists, check what
format it uses (ISO-3166 alpha-3 codes like `CHN`/`TWN`/`KOR` are common in California voter
files, but confirm against the actual data rather than assuming).

### Step 3 — Compute Chinese (Y/N) and Country per row, in this priority order

1. **Korean check first, before anything else.** Several common Chinese-list surnames overlap
   with Korean surnames (Lee/Li, Lim/Lin, Chang/Zhang, Yu). If either of these is true:
   - `birth_place` matches Korea or North Korea (e.g. `KOR` or `PRK`), **or**
   - the first or middle name matches a common romanized Korean given-name pattern (see
     `references/korean-given-names.md`)

   then set `Chinese (Y/N) = "N"` and `Country = "Korea"` — regardless of whether the surname
   also matched the Chinese list. This takes priority over a surname match. Be upfront with the
   user that the birth-place check is reliable (real self-reported data) but the given-name check
   is a low-confidence heuristic — it only catches traditional Korean given names, and misses
   Korean-Americans who use Western first names entirely. Don't silently oversell this column's
   accuracy.

2. **Otherwise, check the surname against the Chinese surname lookup.** If it matches:
   - `Chinese (Y/N) = "Y"`
   - `Country`: if `birth_place` maps to one of the countries the surname list covers, use that
     value directly (real data beats a guess). Otherwise, list **every** country the surname
     matches in the reference list, joined with `"; "` — never collapse a multi-country match
     down to one guess, and never add hedge words like "(ambiguous)" into the cell itself (that
     breaks downstream filtering); the semicolon-joined list is self-explanatory.

3. **Otherwise** (no Korean signal, no surname match): `Chinese (Y/N) = "N"`, `Country = ""`.

Do not add a separate "possible Korean" column — the Korean determination folds directly into
the `Country` field as described above, keeping the output to exactly two new columns.

### Step 4 — Insert columns before the last-name column, not appended at the end

Use the target file's actual column position for last name (don't hardcode an index — read the
header). Insert two new columns immediately to its left: `Chinese (Y/N)`, then `Country`. Every
other existing column stays in its original order and position. This matters for usability —
canvassers scanning the sheet want the match right next to the name, not off in column BF.

Read `/mnt/skills/public/xlsx/SKILL.md` before writing the file — for a large voter file (tens
of thousands of rows), use `openpyxl`'s `insert_cols` on a normally-loaded (non-read-only)
workbook rather than rebuilding the sheet with `pandas`, to avoid losing existing formatting/date
types on the untouched columns.

### Formatting the new columns

Every cell in the two new columns — header and all data rows — gets:
- **Light green fill** (`PatternFill("solid", fgColor="C6EFCE")` — Excel's standard light green,
  readable and not garish at 20,000+ rows).
- **Thin border on all four sides** (`Border(left=Side(style="thin"), right=Side(style="thin"),
  top=Side(style="thin"), bottom=Side(style="thin"))`), applied per-cell.

This is a visual affordance so the two added columns stand out at a glance against the
untouched original columns — don't apply this fill/border to any existing column. Keep the
existing font convention from the rest of this skill (Arial, bold header / size-10 body) on top
of the fill — formatting additively, not replacing font choices already specified above.
Applying styles cell-by-cell across tens of thousands of rows is the correct approach here
(openpyxl has no "apply to column" shortcut that also covers the header), just batch it in the
same loop that writes the values rather than a second pass over the sheet.

### Step 5 — Add a short Match Summary sheet

Prepend (don't append) a sheet with: total rows, count and % matched Y, count relabeled to
Korea (broken down by birth-place-confirmed vs. name-heuristic-only if you can), count of
multi-country rows, and the same methodology + limitations notes as the workflow above — so the
caveats travel with the file, not just this conversation.

## Reference files

- `references/korean-given-names.md` — the curated list of common romanized Korean given-name
  tokens used for the heuristic, plus its known limitations. Extend this list as you find more
  common tokens in future runs, and keep the limitations note current — it's the main thing that
  keeps this heuristic from being oversold to the user.
