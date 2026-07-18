# Korean given-name heuristic (low confidence — read the limitations before using)

## Purpose

Some Chinese-surname-list entries (Lee/Li, Lim/Lin, Chang/Zhang, Yu, and others) are also common
Korean surnames. `birth_place` reliably catches foreign-born Korean voters (country code `KOR`
for South Korea, `PRK` for North Korea — confirm actual codes used in the target file, these are
ISO-3166 alpha-3 as commonly seen in CA voter files but don't assume). It does **not** catch
US-born Korean-Americans, who have no birth_place signal to check. For that population, the only
available signal is the first/middle name — hence this heuristic.

## Token list

Match first/middle name against these tokens (split the name on spaces/hyphens, uppercase,
compare each token — not a substring match, a whole-token match):

**Common male given names / components:** MINJUN, JIHOON, JIHUN, SEOJUN, DOYOON, DOYUN, JUNHO,
SUNGMIN, HYUNWOO, JINWOO, KYUNGSOO, YOUNGHO, DONGHYUN, JAEHYUN, SEUNGHYUN, MINHO, JUNSEO, SIWOO,
EUNWOO, TAEYANG, JOONHO, WOOJIN, HYUNJUN, SANGWOO, YOONHO, KYUNGHO, SEUNGWOO, JAEWON, HYUNSOO,
DAEHYUN, YOUNGJUN, MINSU, MINSOO, JONGHYUN, KWANGHO

**Common female given names / components:** JIWOO, SEOYEON, SOOMIN, SUMIN, YUNA, EUNJI, MINJI,
SUJI, HYUNJI, JIEUN, YERIN, SOYEON, JIYOUNG, HYEJIN, MINKYUNG, SEUNGYEON, YOONJI, EUNSEO, HAEUN,
DAEUN, SEOYUN, NAYEON, YEJIN, JISOO, JISU, HAYOON, CHAEWON, YOOJIN, SOOYEON, EUNKYUNG

**Common standalone syllable-components** (appear alone as a full given name sometimes, not just
as part of a compound): SUNG, HYUN, KYUNG, YOUNG, JUNG, WOO, SOOK, EUN, SEOK, JONG, KWANG

## Known limitations — say these out loud to the user, don't just silently apply the list

- **Low recall.** Most Korean-Americans, especially multi-generation, use Western first names
  (James, Grace, Daniel, Susan...) that are completely indistinguishable from any other
  ethnicity's. This heuristic only catches the subset who have a traditional Korean given name in
  Western records — likely a minority of the true Korean-American population in most US voter
  files.
- **Nonzero false-positive risk.** A few tokens (SUNG, WOO, EUN) are short enough that they could
  coincide with a truncated or misparsed name field. Spot-check before treating this as gospel for
  a large batch.
- **This is not a substitute for birth_place.** Always check birth_place for Korea/North Korea
  first — it's real, self-reported data and should take priority whenever available. Only fall
  back to the name heuristic when birth_place is missing or doesn't resolve it.
- **Extend, don't replace.** If a campaign in a specific region reports names that should be added
  to this list, add them — this list is a reasonable starting point, not a validated model.
