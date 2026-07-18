# Surname sources by country

Check here first. Prefer these over generating a list from memory. Add new sources you find
back into this file (with the query that found them) so future runs of this skill don't
re-derive them from scratch.

## China (PRC)
- **Best source**: Fuxi Institution's "China's 400 Great Surnames" (2013, Yuan Yida & Qiu Jiaru),
  based on Ministry of Public Security registration data. Reproduced in full (rank, character,
  pinyin, % of population, raw count, province with most) on Wikipedia's
  "List of common Chinese surnames" article. Pull the **full list this source provides** (up to
  400), not just the top 100 — a prior run of this skill only captured the top 100; the source
  goes further, so default to the deeper version going forward.
- China's MPS also releases an annual top-100 report (most recent found: 2019/2020 release).

## Taiwan
- **Best source**: Taiwan Ministry of the Interior, Dept. of Population, 2016 household
  registration survey — top 30+ with Mandarin/Hoklo(Minnan)/Hakka/Cantonese romanizations.
  Also reproduced on the Wikipedia "List of common Chinese surnames" page.

## Singapore
- **Best source**: Statistics Singapore, "Popular Chinese Surnames in Singapore" — two published
  lists (an older ~20-name list and a newer, larger ~127-name list), with dialect-origin
  (Hokkien/Teochew/Cantonese/Hakka/Hainanese) noted per name. Romanized forms are
  dialect-based (Tan, not Chen), which matters a lot for matching real voter-roll spellings.

## Philippines
- **Chinese-Filipino specifically**: Forebears.io Philippines genealogical data (2020 figures),
  reproduced on Wikipedia. Many are Hokkien-derived Spanish-orthography transliterations
  (the "grandson" numbering series: Dizon/Samson/Sison/Tuazon/Sayson/Cuizon/Tecson).
- **Filipino broadly (not specifically Chinese)**: most common Filipino surnames are Spanish
  (Santos, Reyes, Cruz, Garcia, etc. — 1849 Claveria decree). Don't use these for
  Chinese-Filipino targeting; do use them if the target ethnic group is "Filipino" generally.

## Malaysia
- **Known gap**: no single official ranked government surname-frequency table located as of
  last check. Malaysia's Dept. of Statistics has not published one publicly. Secondary sources
  (Free Malaysia Today, SAYS, WorldOfBuzz) repeatedly cite Tan, Lee, Lim, Chong, Wong as most
  common but without verified counts — flag as unranked/approximate if used.

## India
- **No centralized national surname-frequency source exists** — India's census does not publish
  a ranked surname list the way China/Taiwan/Singapore do, and surnames vary enormously by
  region, religion, and caste (a "top 100 Indian surnames" list is not a coherent thing the way
  it is for China). Build composite regional lists instead, organized by language/region, e.g.:
  - Punjabi (esp. Sikh) surnames: Singh, Kaur, Gill, Sandhu, Sidhu, Dhillon, Brar, Sekhon, etc.
  - Gujarati surnames: Patel, Shah, Mehta, Desai, Trivedi, Joshi, etc.
  - Tamil/South Indian: often patronymic/initial-based rather than fixed family surnames —
    note this structural difference explicitly rather than forcing a "surname" list.
  - North Indian Hindu (Hindi Belt): Sharma, Verma, Gupta, Agarwal, Yadav, Mishra, Tiwari, etc.
  - Marathi: Deshmukh, Kulkarni, Joshi, Patil, etc.
  - Bengali: Banerjee/Bandyopadhyay, Chatterjee/Chattopadhyay, Mukherjee, Das, Roy, Ghosh, etc.
  Cross-check candidate lists against US Census Bureau "Frequently Occurring Surnames"
  (2010 census surname file) filtered for these names, if precision on US-context spelling
  matters for the canvassing use case — that file shows actual counts of each spelling as
  used by real US residents, which is often more useful for canvassing than a "correct"
  transliteration would be.
- Devanagari is the standard script for Hindi-region names; note that Gujarati, Punjabi (Gurmukhi),
  Tamil, Bengali, etc. all have their own scripts — don't default to Devanagari for the
  "original script" column of a Tamil or Punjabi surname without checking.

## Vietnam
- Surname pool is extremely concentrated: Nguyễn (~40%), Trần, Lê, Phạm, Hoàng/Huỳnh, Phan, Vũ/Võ,
  Đặng, Bùi, Đỗ, Hồ, Ngô, Dương, Lý cover the large majority. A long ranked list adds little
  value; a ~15-20 name list with rough share estimates is usually sufficient and more honest.

## South Korea
- Kim (~20%), Lee/Yi (~15%), Park/Bak (~8%), and a handful of others (Choi, Jung/Jeong, Kang,
  Cho, Yoon, Jang, Lim) cover a large majority of the population — also a concentrated pool
  like Vietnam, not a long-tail-100 situation. Check for South Korean government (Statistics
  Korea / 통계청) census surname releases for precise current figures.

## Canada (Ontario) — bonus overseas diaspora source, not a country of origin
- Shah et al. (2010), "Surname lists to identify South Asian and Chinese ethnicity from
  secondary data in Ontario, Canada: A validation study," BMC Medical Research Methodology —
  data-mined the Ontario health-card registry for validated Chinese-Canadian AND South-Asian-
  Canadian surname lists. Useful cross-check even for US-targeted lists since it's a real,
  large, English-language-context dataset of diaspora spellings.

## General fallback when no country-specific registry exists
US Census Bureau's "Frequently Occurring Surnames" release (from the decennial census, most
recently 2010) lists real surname counts as actually used by US residents, is public, and
covers every country of origin at once — filter it for candidate names from whatever regional/
diaspora list you've built, to confirm the spellings actually show up in US records and see
real counts. This is often the most directly useful source for a *canvassing* list specifically
(as opposed to a country-of-origin's own internal ranking), since it reflects the Americanized
spelling that will actually appear on a voter roll.
