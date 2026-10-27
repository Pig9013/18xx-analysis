[中文](README.md) · **English** · [Français](README.fr.md) · [日本語](README.ja.md) · [Русский](README.ru.md)

# 18xx Game Data Analysis · Workflow and Reports

> Turning hundreds of public [18xx.games](https://18xx.games/) logs into **publishable, numerically traceable** in-depth analysis reports.
> Every table comes from per-game parsing and per-player reconciliation; every conclusion carries **count + rate + win rate + case ID**; where there is no data, we say there is none.

**Repository version: v1.0.2** (2026-10-25)

**Current version**

| Component | Version | Notes |
|:--|:--|:--|
| skill `18xx-analysis` | **v1.4.1** | Five-stage workflow (rules → intake → parse → statistics → report) + 18 iron rules |
| Reports | 4 titles | 1846 (672 games), 1830 (532 games), 1862 (419 games), 1880 China (341 games) |

---

## 1. Report index

| Title | Games | Player-games | Reconciled | Version | Main report | Other deliverables |
|:--|--:|--:|:--|:--|:--|:--|
| **1846** | 672 | 2,740 | **2740/2740 = 100.0%** | v1.1 | [Main report (Chinese)](<works/1846/1846深度分析（672局）.md>) | [Briefing deck, 55 slides](<works/1846/1846深度分析（672局）.pptx>) · [English edition](<works/1846/1846 Deep Analysis (672 Games).md>) · [EN glossary](<works/1846/1846_EN_GLOSSARY.md>) |
| **1830** | 532 | 532 | **532/532 = 100%** | v1.1 | [Main report (Chinese)](<works/1830/1830深度分析（532局）.md>) | — |
| **1862** | 419 | 1,572 | **1511/1572 = 96.1%** (median residual £203, explained) | v2.5 | [Main report (Chinese)](<works/1862/1862深度分析（419局）.md>) | [Table-side quick reference (one page)](<works/1862/1862实战速查（单页）.md>) |
| **1880 China** | 341 | 1,564 | **1564/1564 = 100.00%** (residual 0) | v1.3 | [Main report (Chinese)](<works/1880China/1880China深度分析（341局）.md>) | [Briefing deck, 48 slides](<works/1880China/1880China深度分析（341局）.pptx>) · [English edition](<works/1880China/1880China Deep Analysis (341 Games).md>) · [EN glossary](<works/1880China/1880China_EN_GLOSSARY.md>) |

> The 341 games of 1880 are the **main statistical sample**: 384 games cut from 6 source files → 358 after deduplication → 341 after removing the 17 games ended by "manual vote".

---

## 2. Method (summary)

| Stage | What it does | Gate |
|:--|:--|:--|
| **S0 Rules** | Read the English rules of this title clause by clause and list the nine mechanical differences from the titles already analysed | A rule error = all wrong |
| **S1 Intake** | Split three kinds of source file (single game / bound volume / no game header); **deduplicate by content sha1**; canonical numbering + `_mapping.csv` | Sample is unique after deduplication |
| **S2 Parse** | Write a title-specific bookkeeping state machine and rebuild cash / holdings / net worth player by player | **Reconcile player by player against the official final settlement** |
| **S3 Extract** | Reuse the parse state machine, observe only and change nothing, emit `_full.json` in bulk | Reconciliation rate written into the report header |
| **S4 Statistics** | 14 topic scripts (opening train mix, train rush, treasury, turn order, price dumping, ratings…) | Every number traces back to a script output |
| **S5 Report** | Fixed skeleton: data and reconciliation → player layer → mechanics and behaviour → quick reference → method and limitations | Pre-delivery self-check list |

**A few hard definitions**:

- Top-two rank rating: 1st +2, 2nd +1, 3rd and below −(rank−2); only "top two vs. not top two" comparison pairs are built;
- Normalised rank =(rank−1)/(players−1); **average rank per game is not compared across different player counts**;
- The top-two rate is computed only for games with ≥4 players; cells with n<20 are for reference only;
- Random attributes are not carried across games (in 1862 the operating licences are re-bound every game and must be split into "company × licence");
- Correlation ≠ causation; every suspected causal link is listed separately.

The full workflow and the 18 iron rules are in the [skill](<skills/18xx-analysis/SKILL.md>).

---

## 3. Player anonymisation

This is mandatory for **every deliverable** (since 2026-09-24):

- **The TOP group keeps real names** (high-level players in public games, so traceability is preserved);
- **The MID / BOT groups are always recorded as "PlayerX"** (`PlayerX` in the English edition), numbered by descending strength score, MID block first and BOT block after;
- The tier boundaries are independent per title (1846 by the printed list, 1880 by BT≥2.0, 1830 by BT≥1700/≤1420, 1862 by BT≥1700);
- Before delivery run `anonymize_18xx.py --check`; regenerate decks with `build_*_ppt.py` (never hand-edit the XML).

---

## 4. Data sources and acknowledgements

**All game data comes from [18xx.games](https://18xx.games/) and the players who play there** — their freely published play records are what made these analyses possible. Without those public logs this project would have no data at all.

- Acknowledgements appear at the beginning and at the end of every report;
- The raw game logs and the original parsing scripts are **not in this repository** (size and data-boundary reasons); every number in the reports is tagged with the script and definition behind it, and anyone who needs to verify can open an Issue.

---

## 5. Copyright and use

- **Analysis text, charts, slides**: original work by Pig9013; **when reposting, credit the author and the source; commercial use is prohibited**;
- **Game data**: the rights belong to 18xx.games and the respective players; this repository only cites it statistically and does not reproduce complete logs;
- Rule citations follow the original English rules; the Chinese translation is used for cross-checking only, and any mistranslation found is pointed out in the report.

---

## 6. Repository layout

`@
skills/18xx-analysis/    SKILL.md + references/ (templates, data traps, anonymisation, slide delivery…)
works/1846/              main report + deck + English edition + EN glossary
works/1830/              main report
works/1862/              main report + table-side quick reference
works/1880China/         main report + deck + English edition + EN glossary
tools/verify_repo.py     static self-check before release
`@

---

*Analysis: Pig9013 ｜ Model: Deepseek-flash ｜ Last updated: 2026-10-25*
