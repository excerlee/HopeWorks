---
name: hopeworks-canvassing-surname-lists
description: >
  Build a surname list for ballot canvassing or community/language-access outreach targeting a
  specific ethnic or immigrant-origin group in a specific city. Use whenever the user gives a
  city (or metro area, county, or state) plus a target ethnic group (e.g. "Chinese", "Indian",
  "Vietnamese", "Korean", "Filipino") and wants likely surnames for canvassing, mailers,
  phone-banking, translated-materials targeting, or voter-file matching. Also trigger for "help
  me find Chinese last names in Fremont", "build a surname list for Indian voters in Edison NJ",
  or "biggest immigrant groups in Alhambra and their common last names". Four-step workflow -
  map the city to its metro area/CBSA or state; find the top 5-10 countries of origin for that
  ethnic group there via Census ACS place-of-birth data; build a surname list per country with
  every surname the source publishes (not capped at a round number), with both the
  Americanized/romanized form and the original-script form as separate columns; deliver as a
  spreadsheet.
---

# Canvassing Surname Lists

Builds a surname list a campaign or community organization can use to identify likely voters/residents of a target ethnic or national-origin group in a specific place, for canvassing, mailers, phone-banking, or translated-materials targeting (e.g. language-access outreach under VRA Section 203). Same underlying workflow as any surname/name-frequency research request — this skill just fixes the geography → countries → surnames pipeline so it's fast and consistent.

**Ethical/legal framing (state this to the user once, briefly, don't belabor it):** surname-based outreach lists are a standard, legal campaign and civic-engagement technique — used for translated mailers, in-language canvassing scripts, and community coalition building. They are probabilistic, not identity verification: a surname match is a *hint* to prioritize outreach, never proof of someone's ethnicity, citizenship, or voter eligibility. Don't use these lists to include/exclude anyone from services they're entitled to, and always canvass respectfully regardless of whether a name match was right.

## Workflow

### Step 1 — Map the city to its metro area / state

Campaign-relevant immigration data isn't published at the city level for most cities — it's published at the **Core-Based Statistical Area (CBSA/"metro area")** or **state** level by the Census Bureau. First step is always to resolve this mapping:

1. If the user already named a metro area or state, skip to Step 2.
2. Otherwise, web search `"<city>" metropolitan statistical area CBSA` to find the metro area name (e.g. "Fremont, CA" → "San Francisco-Oakland-Berkeley, CA Metro Area", or more precisely the "Oakland-Fremont-Berkeley, CA Metropolitan Division" if it exists as a subdivision).
3. Note the state too — for smaller cities/towns where metro-level immigrant-origin data is too sparse or unavailable, state-level data is the fallback.
4. Tell the user which geography you're actually pulling data for (city-level data is rarely available directly — be upfront that you're using the metro/state as a proxy).

### Step 2 — Find the top 5-10 countries of origin for the target ethnic group

Goal: rank countries of birth for foreign-born residents in that metro/state who'd plausibly fall under the user's target ethnic group.

1. **Map the ethnic group to relevant countries of birth first** — this is not always 1:1. See `references/ethnic-group-to-countries.md` for the expansion logic (e.g. "Chinese" → China, Taiwan, Hong Kong, Singapore, Malaysia, Vietnam, Indonesia, Philippines all contribute ethnic-Chinese immigrants; "Indian" → India plus Indian-diaspora "twice-migrant" source countries like Guyana, Trinidad, Fiji, Kenya/Uganda, UK, Mauritius, South Africa).
2. Web search for the specific metro/state's foreign-born population by country of birth. Good query patterns:
   - `"<metro area>" foreign born population country of birth Census ACS`
   - `data.census.gov "<metro/state>" place of birth foreign born`
   - Try fetching `data.census.gov` ACS Table **B05006** (Place of Birth for the Foreign-Born Population) for the relevant geography if it's directly accessible; otherwise rely on news/demographic reports that cite this table, or state demographic-center reports (many states publish their own breakdowns, e.g. California Dept. of Finance, NY State Comptroller).
3. From the countries relevant to the target ethnic group (Step 2.1), rank by immigrant population size in that geography. Take the top 5-10; note the rest exist but push them to a "backlog" note the same way — this keeps the deliverable focused on where canvassing effort will actually pay off.
4. If precise metro-level numbers aren't publicly available (common for smaller metros), fall back to state-level ranking and say so explicitly rather than guessing city-level numbers.

### Step 3 — Build the surname list per country

For each of the top 5-10 countries, **include every surname the source publishes — do not cap the list at a round number like 50 or 100.** If China's Fuxi Institution study covers 400 surnames, include all 400, not just the top 100. For India, this means including every surname across every regional/language sub-list you compile (see the failure-mode note below) — don't trim a large composite India list down to a round number either, the same "as complete as the source allows" rule applies whether the source is one centralized government registry or several regional ones stitched together. If a source only reliably covers 20 names, 20 is the right length and padding it further with lower-confidence guesses is worse than a shorter, fully-sourced list. The right length is "as complete as the best available source," not a target row count. **Prioritize existing published sources over generating names from memory** — see `references/surname-sources-by-country.md` for a working catalog of good sources (government registries, census surname-frequency releases, academic surname studies) for common target countries, organized by which ethnic groups they serve. Update that file with new sources as you find them in future runs of this skill, and note there when a source goes deeper than what a prior run of this skill captured (e.g. "found the full 400-surname list, not just top 100") so future runs pull the fuller version by default.

Required columns for every country's table:

| Column | Notes |
|---|---|
| Rank | within that country's list |
| Romanized / Americanized form | the spelling most likely to appear on a US voter roll or mailing list — this is often the dialect-based or English-transliterated form (e.g. "Tan" not "Chen" for a Hokkien-origin Chinese-Filipino; "Singh" as literally spelled), NOT necessarily standard Pinyin/IAST transliteration |
| Original-script form | pinyin (with tone marks) for Chinese, Devanagari/regional script for South Asian names, etc. — always as its own column, never merged into the romanized column |
| Country / region of origin | which country's list this row belongs to |
| Language / dialect | e.g. Hokkien, Cantonese, Hindi, Punjabi, Gujarati, Tamil — helps canvassers pick the right in-language materials |
| Source | short citation |

**Formatting rules for the Romanized column (important for voter-file matching):**

- **One spelling per row, always.** Never combine alternate spellings with "/" or "or" in a single cell (not "Chang / Zhang", not "Lee / Li"). If a name has multiple valid romanizations, split it into separate rows — one per spelling — each carrying its own rank/source. The reason: this column will get matched against a flat voter-file text column, and a cell like "Chang / Zhang" will never exact-match anything. Every row must be independently usable in a plain lookup or filter.
- **Plain ASCII only in the Romanized column — no diacritics, no tone marks.** US voter rolls and mailing lists are plain ASCII; a name with a tone mark or accent (Vương, Nguyễn, Lǚ) will not match how it actually appears in US records. Strip all diacritics for this column (Vương → Vuong, Nguyễn → Nguyen).
- **Diacritics go in the Original-script column instead, paired with the character.** Format as `character(diacritic form)`, e.g. `王(Vương)` for Vietnamese, so the linguistic detail is preserved without contaminating the matchable column. This applies to any romanization system that carries tone/diacritic marks — Vietnamese Hán Việt, Taiwanese Hoklo POJ, Hakka PFS, etc. — not just Vietnamese.
- Splitting dual spellings into separate rows, and including a source's full list rather than a truncated top-N, both push row counts well past any round-number expectation — that's fine and expected, don't trim rows back to hit one.

Common failure mode to avoid: for ethnic groups without a single centralized surname registry (e.g. Indian — surnames vary enormously by region, religion, and caste, and India's census doesn't publish a national surname-frequency list the way China's or Taiwan's governments do), don't force a single flat *ranked* list. Instead compile per-major-language-region sub-lists (e.g. Punjabi Sikh surnames, Gujarati surnames, Tamil surnames, South Indian vs. North Indian patterns) and give each its own tab (see Step 4) — splitting India into several region-specific tabs is the right move here, not a shortcut to avoid; be explicit in a note atop each sheet that it's a composite regional list, not an official national ranking. Same caution applies to Vietnamese (extremely concentrated — ~40% of the population shares the surname Nguyễn, so a frequency list is nearly useless for canvassing on its own; note this rather than padding the list with low-confidence names just to make it look bigger) and to any group whose naming conventions don't map cleanly onto "one flat ranked list."

### Step 4 — Deliver as a spreadsheet

Read `/mnt/skills/public/xlsx/SKILL.md` before building the file. Structure:
- One **Read Me** sheet: geography resolved, ethnic group → countries mapping used, sources, and the ethical-use note.
- One **Country Ranking** sheet: the ranked list of countries with population estimates and citations, exactly like the Country Ranking sheet pattern used in prior runs of this skill.
- **At least one sheet per country — never combine multiple countries' surnames onto one shared tab.** For a country with a single centralized source (China, Taiwan, Philippines), one tab per country is natural. For a composite-source country like India, splitting further into **one tab per region/language** (e.g. "India - Punjabi", "India - Gujarati", "India - Tamil") is fine and often clearer than one large India tab — the point is never to merge unrelated groupings together into a single flat tab, not to minimize tab count. When in doubt, more tabs with a clear, narrow scope each beats fewer tabs with mixed contents.
- If useful for canvassing, offer a **Combined / Master** sheet with all countries' names in one filterable table (Rank, Name, Original Script, Country, Language, Source columns) — ask the user if they want this before building it, since combining changes how they'll use the file (single voter-file lookup column vs. per-country breakdown). This Combined sheet is an additive convenience utility, not a replacement for the per-country/per-region tabs above — it doesn't relax the "don't cram everything together" rule for the primary sheets, it's simply a second, clearly-labeled view for a different use case.

### Step 5 — Be upfront about precision limits

Always tell the user, briefly, in the delivered file's Read Me sheet:
- Surname matching has real false-positive/false-negative rates (a "Lee" could be Chinese, Korean, or of many other origins; many people of the target ethnicity have surnames not on any list, especially post-immigration name changes or mixed-heritage families).
- This is a targeting *aid*, not a determination of anyone's identity — canvassers should never assume based on a name alone.

## Reference files

- `references/ethnic-group-to-countries.md` — how to expand an ethnic group label into the right set of countries of birth to search for, with worked examples (Chinese, Indian, Vietnamese, Korean, Filipino).
- `references/surname-sources-by-country.md` — running catalog of good existing surname-frequency sources by country (government registries, academic studies, census releases). Check here first before web-searching from scratch; add new sources you find back into this file.
