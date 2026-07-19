# HopeWorks

Practical civic-tech skills for campaign teams and community organizers.

This workspace packages reusable skill prompts that help with multilingual outreach,
data-informed canvassing, and targeted voter-file enrichment.

## Skills

### 1) Canvassing Surname Lists
Directory: `hopeworks-canvassing-surname-lists.skill/`

Builds high-coverage surname lists for a target ethnic group in a target geography.
It maps city -> metro/state context, identifies likely countries of origin, and outputs
structured surname tables (romanized plus original script) that are ready for outreach,
mailers, and voter-file matching workflows.

### 2) Canvassing Training Materials
Directory: `hopeworks-canvassing-training-materials.skill/`

Generates a complete volunteer training kit from campaign inputs:
- pocket-sized cheatsheets,
- a 20-minute training slide deck,
- curated YouTube training videos.

Content is localized for district issues and language/community context so volunteers can
use it immediately in real canvassing shifts.

### 3) Match Chinese Voters
Directory: `hopeworks-match-chinese-voters.skill/`

Matches voter-file surnames against a Chinese surname reference list and adds labeling
columns for quick field use. Includes Korean false-positive handling via birthplace and
given-name patterns to improve practical precision.

## Potential To-Do List

1. Add a data source for city-level ethnic-group percentages to validate surname-matching results.
2. Document an end-to-end workflow for using surname-list outputs to power `match-<ethnic>-voters` skills.
3. Create `hopeworks-match-indian-voters.skill` and `hopeworks-match-hispanic-voters.skill`.
