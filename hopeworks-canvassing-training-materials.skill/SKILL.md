---
name: hopeworks-canvassing-training-materials
description: Generate a full canvassing volunteer training kit — a pocket-sized cheatsheet, a 20-minute group training slide deck, and a curated list of YouTube training videos — from a candidate's campaign website plus a target district and target voter ethnic group. Use this skill any time the user is a campaign or canvassing team asking to prepare, train, or brief volunteers for door-knocking/canvassing, even if they only ask for one of the three deliverables (cheatsheet, training deck, or training videos) rather than all three. Trigger on phrases like "canvassing training", "volunteer cheatsheet", "door-knocking script", "canvass packet", "walk sheet talking points", "phone bank script", or requests to prepare volunteers of mixed age/language background for a local campaign. Especially relevant for local/municipal Bay Area-style campaigns with multilingual, multigenerational volunteer pools (students through seniors, some ESL speakers).
---

# Canvassing Training Materials Generator

Builds a three-part volunteer training kit for a political campaign:
1. **Cheatsheet** — half-A5 (quarter-A4), double-sided, pocket-sized field reference
2. **Training deck** — 20-minute group training PPT
3. **Video list** — curated YouTube canvassing training videos matched to the audience

All three are derived from the candidate's website plus two targeting inputs: **district** and **target voter ethnic group/language**.

## Step 0: Gather inputs

Required before starting:
- **Candidate website URL** (or a doc/page with candidate bio, platform, key issues)
- **Target district** (city, ward, or precinct — needed for local issue framing and video geo-relevance)
- **Target voter ethnic group(s)** (needed for language + cultural framing)

Also useful, ask only if not already stated and it would clearly change the output:
- Volunteer age mix (default: assume the full range described by the user — middle/high schoolers through 50s-60s — and write for the widest common denominator: plain language, short sentences, no jargon)
- Which deliverable(s) they want right now (default: all three)

**Candidate pronouns — verify, don't guess from the name.** Names don't reliably indicate gender, especially across cultures/transliterations, and getting this wrong on volunteer-facing materials is embarrassing for the campaign. Check the candidate's own website/bio and photos for pronoun or gendered-language cues (e.g. "he/his," "she/her," a spouse referred to as "husband"/"wife") before writing anything. If the site doesn't make it clear, ask the user directly in Step 0 rather than assuming — a single quick question ("Is [Candidate] a he or she, for the materials?") is worth it before drafting content that repeats the wrong pronoun a dozen times across three documents.

Don't over-ask — if the user gave candidate site + district + ethnic group, proceed. Use `ask_user_input_v0` for at most one round of missing-input questions, not more.

## Step 1: Research

1. **Candidate**: `web_fetch` the campaign website. If the site blocks automated fetching (robots disallowed), fall back to `web_search` on the candidate's name + office + district — search snippets and other coverage (local news, endorsement pages, LinkedIn/social) can usually reconstruct the same information.

   The two things every downstream deliverable is built around, so extract these deliberately rather than just skimming for a bio:
   - **Why they're running** — the candidate's own framing of their motivation/mission (often an "About" or "Why I'm Running" section, or the opening lines of the homepage). Capture this close to their own words/framing (paraphrased, not quoted at length) — this becomes the opening hook on the cheatsheet and the "meet the candidate" slide.
   - **Priorities / what they stand for** — the concrete platform points, ideally as a short list (e.g. "Issues," "Priorities," "Platform" page). Capture each as a plain-language one-liner a volunteer could say on a doorstep, not the campaign's original policy-memo phrasing. Aim for 3-6 priorities; if the site lists more, pick the ones most relevant to the target district.

   Also grab if available: candidate name, office sought, one-line bio/background (prior roles, community ties — especially any tie to the target ethnic group's community, which is useful doorstep credibility), campaign slogan, election date. If the site is thin on any of this, `web_search` "[candidate name] [office] [district]" to fill gaps — paraphrase per copyright rules, never quote more than one short phrase per source.

   While you're on the site, confirm pronouns from real cues (bio language, photos, family references) — see the pronoun note in Step 0. Do not infer gender from the name alone.
2. **District context**: `web_search` for the district's key local issues (e.g. housing, schools, transit, public safety) so talking points feel locally grounded rather than generic. If the campaign site already covers this, skip.
3. **Ethnic group / language mapping**: identify the primary language(s) associated with the target ethnic group in that Bay Area locality. See `references/language-mapping.md` for a starting table (Chinese, Vietnamese, Korean, Filipino, Indian/South Asian, Mexican/Latino, etc.) — verify/adjust with a quick search if the group isn't listed or the district has a distinctive dialect mix (e.g. Cantonese vs. Mandarin varies by city/generation in the Bay Area — check which dominates for that district if possible, and when unsure, default to including both).
4. **Cultural framing notes**: quick search for any culturally-specific canvassing etiquette worth knowing (e.g. addressing elders formally, common concerns like property tax/Prop 13 for immigrant homeowners, distrust of unsolicited visitors) — keep this to a short practical list, not a stereotype-laden generalization. Stick to well-documented civic-engagement patterns, not assumptions about individuals.

## Step 2: Build the cheatsheet

Read `/mnt/skills/public/docx/SKILL.md` before producing this file — follow its formatting rules exactly.

Format target: **half of a double-sided A5** — i.e., when the final page is cut/folded, each volunteer gets a compact card roughly 4.1" x 5.8" (A6-ish) or a half-A5 slip. The simplest reliable way to hit this: lay out a single **A4 page in a 2x2 grid** (four A5 quadrants) — top two quadrants = side A content x2, bottom two = side B content x2 — so that when the sheet is cut in half and folded (or cut into quarters), each volunteer gets one complete double-sided A5 card, and one A4 print sheet yields 2 cards. Also offer a plain single-card **A5-page-size** doc as an alternative for volunteers who'll just print single cards.

Content (keep it SHORT — this is a glance-at-the-door reference, not a script to read verbatim):
- **Side A — Opening**
  - Candidate name, office, one-line "why I'm running" hook (from Step 1.1)
  - 3-4 sentence opening intro volunteers can say (in their own words) — first-person, natural, e.g. "Hi, I'm a volunteer for [Name], running for [office]. Got a minute to talk about [top local issue]?"
  - 3-4 bullet talking points drawn straight from the candidate's stated priorities (Step 1.1), in plain language, one line each — this is what makes the card specific to this candidate rather than generic
- **Side B — Handling the conversation**
  - 3-4 common questions/objections with 1-line responses ("Q: What's their record on X? A: ...")
  - What to do if: not registered / already decided / hostile / not home (short scripted lines)
  - Closing ask (e.g. "Can we count on your vote / can I leave this flyer / would you like a yard sign?")
  - Volunteer safety/logistics reminders (buddy system, don't enter homes, log the door, emergency contact) — 2-3 lines max
  - Campaign contact info / QR code placeholder line if the campaign has one

### Produce two full cheatsheets, not one card with snippets

Default language is English. Whenever a target ethnic group/language is given (or is inferable from Step 1.3), produce **two separate, complete cheatsheet documents**, each with the full Side A + Side B content above, laid out identically:

1. `cheatsheet-english.docx` — fully in English
2. `cheatsheet-[language].docx` — fully translated into the target language (not just a few translated lines bolted onto the English card). Same structure, same length, same layout, so a volunteer using the translated card has everything an English-speaking volunteer has, not an abridged version.

Keep the file naming clear (e.g. `cheatsheet-mandarin.docx`, `cheatsheet-spanish.docx`) so volunteers can grab the right one. If more than one language is in play for the district (see `references/language-mapping.md`), produce one full cheatsheet per language rather than combining languages on a single card — confirm which languages to prioritize with the user only if there are more than 2 worth covering.

Use large, legible type (11-12pt body minimum) since this gets read on a doorstep, and generous white space. Save all cheatsheet files to `/mnt/user-data/outputs/`.

## Step 3: Build the training deck

Read `/mnt/skills/public/pptx/SKILL.md` before producing this file — follow its formatting rules exactly.

Target: **20-minute** in-person group training, ~12-15 slides (roughly 1.5 min/slide including discussion). Structure:

1. Title — candidate, office, "Canvassing Volunteer Training"
2. Why canvassing matters (1 min) — quick motivating stat/framing, not preachy
3. Meet the candidate (2 min) — bio + why they're running (Step 1.1), in the candidate's own framing
4. Top issues to know (3 min) — the priorities/platform points from Step 1.1, one per bullet, plain language — this is the core content volunteers need to represent the candidate accurately
5. District-specific context (2 min) — what matters locally, tie to target neighborhoods
6. The doorstep script walkthrough (3 min) — mirrors the cheatsheet Side A, shown large
7. Handling objections (3 min) — the Q&A pairs from the cheatsheet, presented for group repetition/practice
8. Cultural & language notes (2 min) — practical etiquette + note on translated materials for the target group; frame respectfully, as "helpful context," not stereotypes
9. Safety & logistics (2 min) — buddy system, routes, what to log, do's/don'ts, what to do if uncomfortable
10. Role-play prompt slide (2 min) — pair up, practice the opening + one objection out loud
11. Wrap-up / questions / where to pick up materials & routes

Design notes: read `/mnt/skills/public/frontend-design/SKILL.md` if available for visual polish, but keep this practical over flashy — big text, minimal text-per-slide (this is a spoken training, slides are prompts not scripts), one clear visual/photo anchor per section where sensible. Given the mixed age audience (teens through 60s), avoid dense paragraphs and avoid overly gimmicky design. Save to `/mnt/user-data/outputs/`.

## Step 4: Find YouTube training videos

Use `web_search` (queries like "canvassing training video volunteers", "door to door canvassing tips video", "[language] canvassing training video" if a same-language training video would help ESL volunteers) then `web_fetch` promising youtube.com links to confirm they're real, on-topic, and reasonably current/high-quality (check view count / channel credibility where visible). Prioritize, in order:
1. Any video specifically about canvassing for local/municipal campaigns (not just federal-scale)
2. General nonpartisan canvassing skills training (opening a conversation, handling rejection, safety)
3. If available, a video in the target group's language, or one that models canvassing to that community
4. Something short enough to actually fit in a training session (under ~10 min) if possible, noting run time

Present 3-6 videos as a short list: title, link, run time if known, one-line note on why it's a good fit for this audience (age range / language / district relevance). Do not embed or reproduce video transcripts/captions — link only.

## Step 5: Deliver

- Save the cheatsheet(s) (docx — one per language, see above) and deck (pptx) to `/mnt/user-data/outputs/`, then use `present_files` to hand them all to the user.
- Present the video list inline in chat with **copy-able URLs** (formatted one per line, not in markdown link syntax) — training coordinators often need to paste these into emails or Slack, and copy-able plain URLs are more useful than clickable markdown. Lead with a short intro sentence noting any partisan leanings or other caveats about each video.
- Briefly note in your reply which language(s) got a full cheatsheet and why, so the user can correct you if the district's language mix is off — don't ask this as a formal clarifying question, just state the assumption inline.

## Notes

- If the user gives you multiple target ethnic groups or a multilingual district, make one cheatsheet per language pairing rather than cramming 3+ languages onto one card — offer to do this and ask which languages to prioritize only if there are more than 2 candidates worth covering.
- Never invent a candidate's positions — if the website doesn't cover an issue, don't guess; either search for a credible source or note it as "confirm with campaign" on materials rather than fabricating a stance.
- Keep all copy nonpartisan-toned and factual about the candidate — this is training material, not attack material; avoid disparaging opponents even if the campaign site does.
