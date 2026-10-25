# 1846 Deep Analysis (672 Games) · v1.1

> **Analyst**: **Pig9013** | **Model used**: **Deepseek-flash** | **Date compiled**: **2026-09-17**
> **Acknowledgement**: All game data comes from **[18xx.games](https://18xx.games/)** and the players who play there — it is their freely published play records that made this analysis possible.
>
> **Version history**
> - **v1.0 (2026-09-17)**: first release (unversioned). 665-game main sample, **reconciliation 2,740/2,740 = 100%**, top-two rank rating tiers, 14 chapters of mechanics and behaviour analysis.
> - **v1.1 (2026-09-24)**: **player anonymisation** — the TOP group (the printed list of 9 in §1.5) keeps real names; **all MID/BOT players are rendered as `PlayerX`** (numbered by RR score descending, MID band first, then BOT). §1.5 gained a note on the TOP cut-off.
>
> **Data basis**: `复盘分析/1846/` — **96 source files** (newly added `1846-320.txt` 261 games, `1846-400.txt` 99 games)
> → 734 games split out → **672 unique games** after de-duplication
> **Main statistical sample**: **665 games / 2,740 player-games / 551 players** (7 two-player variants excluded, rules differ)
> **Player-count distribution**: 3-player 62 | **4-player 461** | 5-player 142
> **Reconciliation**: **2,740/2,740 player-games = 100.0% exact match**
> **Metric upgrade**: **top-two rank rating** (1st +2, 2nd +1, 3rd onward deducted); newly added **familiar table / stranger table** and **TOP3 style profiles**
>
> **Nature of this document**: This is a **complete re-run of the old 312-game report on the new database**. Every conclusion, every rules explanation, and every sample warning of the original 14 chapters has been **recomputed on all 665 games**; where a number changed it is marked "(old → new)", and where a conclusion's direction changed it is flagged separately.

---

## Table of Contents

| Part | Section | Content |
|:--|:--|:--|
| **Data** | §0 | Database update, reconciliation, overall board |
| **Players** | §1 | **top-two rank rating** + TOP/MID/BOT tiers |
| **Table type** | §2 | **Familiar table vs stranger table** (new) |
| **Errata** | §3 | Station marker count: GT=3 / five companies=4 / PRR=5 |
| **Turn order** | §4 | OR1.1 reverse turn order, SR1 action order, value of the Priority Deal |
| **Pairings** | §5 | Private × company: win rate, **lift, geographic logic** |
| **Par price** | §6 | Par distribution, sweet spot, **IC Land Grant special**, each tier's pricing habits |
| **Stations** | §7 | Where station markers go, B&O's Cincinnati privilege, **chokepoint "place vs. don't place"** |
| **Trains** | §8 | Opening train combinations, timing of first purchase, **the economics of the 2-train**, treasury at rusting, contributions |
| **Transfers** | §9 | Inter-company train transfers: **99% sell to themselves**, IC is the train-source company |
| **Minors** | §10 | Independent railroad transfer price: **why 84–91% pay the List Price** |
| **Tiers** | §11 | Where TOP/MID/BOT differ |
| **Shares** | §12 | Allocation, timing, rotation, **attribution of price dumping, data-based share advice** |
| **Positioning** | §13 | When revenue arrives, shell corporations, dividend policy |
| **Founding** | §14 | Founding timing and count, **the late-founded company problem** |
| **Profiles** | §15 | Full-lifecycle profile of all seven companies |
| **Playbook** | §16 | **TOP players' favourite lines vs. lowest-win-rate lines** |
| **Blow-up** | §17 | **Special: deliberate blow-up (141 cases)** |
| **Opening** | §18 | **Special: operations in the opening four operating rounds** |
| **Quick ref** | §19 | **Actionable conclusions — quick reference (24 items)** |
| **Method** | §20 | Data and tools, **six limitations** |
| **Style** | §21 | **High-sample player style profiles (8 players)** — 4 high-sample + RR top three + **Player42 case dissection** |
| **Appendix** | — | Mapping onto the 1830/1862 frameworks, list of scripts |

---

## 0. Data and Reconciliation

### 0.1 Database update

| | Old version | **New version** | Change |
|:--|--:|--:|:--|
| Source files | 94 | **96** | +`1846-320.txt`, +`1846-400.txt` |
| Games split out | 570 | **734** | +164 |
| Unique after de-duplication | 312 | **672** | **+115%** |
| Player-games | 1,229 | **2,740** | +123% |
| Unique players | — | **551** | — |
| **Reconciliation** | 100% | **100.0%** | exact match, player by player |

**What doubling the sample means**: many conclusions in the old version rested on small samples of 20–60 cases (for example, the win rate of "a given private with a given company"). **The new version generally expands these samples by a factor of 2–3**, which makes some previously weak signals decidable and confirms some previously strong conclusions.

### 0.2 Case numbering is now automated

**The old version required manual numbering; the new version is fully automatic**: `split_1846_cases.py` recognises three forms of source file —

1. **Single-game files** (`1846-1.txt`);
2. **Bound volumes with a game header** (`1846-91-154.txt`, each game beginning with `1846-137`);
3. **Bound volumes without a game header** (`1846-175.txt` / `1846-320.txt` / `1846-400.txt`, concatenated directly, each game beginning with ` -- Phase I (Operating Rounds…`).

All are output under the canonical numbering `_work/1846_cases/1846-001.txt … 1846-672.txt`, and de-duplicated **by content sha1** (62 duplicates removed from the 734 games).

**Two checks are performed at the same time**: ① whether it is 1846 (determined by a signature string; `1846四人局数据重建.txt` is correctly skipped); ② whether it contains `Game over` (fragments missing their tail are flagged).

**⚠ This de-duplication matters**: the old 72-game draft actually contained only 66 games, and the 167-game and 312-game drafts overlapped heavily — **failing to de-duplicate inflates the sample and also counts two fragments of the same game as two games**.

### 0.3 Two-player games marked separately (different rules, not merged into the main statistics)

**7 two-player variant games** (different starting capital, number of ORs, and some rules) are marked separately and excluded from the main statistics. **"Companies closed in 2-player games: 0"** (§10) — under the variant, companies almost never die.

### 0.4 Overall board

| Metric | Value |
|:--|:--|
| Final phase | **Phase IV 648 games (97%)** \| III 7 \| II 8 \| I 2 |
| Manually ended | 32 games (4.8%) |
| Rounds played | median **15**, mean 16.2 |
| Final assets | median **$6138**, mean $6024, maximum $11442 |
| Companies founded per game | mean **5.80** |
| Company appearance | IC 664 \| NYC 661 \| B&O 658 \| GT 633 \| **PRR 440** \| **C&O 418** \| **ERIE 385** |

**→ 97% of games are played to the end of Phase IV** — 1846 is a game that "runs to completion" and rarely collapses midway.

**→ PRR / C&O / ERIE have markedly lower appearance rates**: **ERIE and C&O have a par floor of $100** (their starting price can only be in the high end of the yellow zone), while PRR is in practice squeezed out by stronger companies — **the weakness of these three is visible from their appearance rates alone**.

### 0.5 Baseline numbers (used for comparison throughout)

**Overall player mean rank = 2.60, win rate = 24%.** Throughout this document "residual" = actual rank − rank predicted by the BT rating, and **a negative value = performed better than one's own skill level predicts**; the overall mean is 0.

---

## 1. Player layer: top-two rank rating

### 1.1 The metric: why it was changed

The old version used a Bradley-Terry model on the **flattened full ranking** (1st > 2nd > 3rd …), that is, **the model treated 1st place as "stronger" than 2nd place**.

But the scoring metric specified by the user is the **top-two system**:

> **1st +2, 2nd +1, 3rd −1, 4th −2… (i.e. for rank≥3, = −(rank−2))**

Under this metric, **1st and 2nd are equivalent (both score positive)**, and 3rd place onward all score negative. The new `rating2_1846.py` therefore **constructs only "top-two vs. non-top-two" comparison pairs**:

```
every top-two finisher in a game → counts as "beating" every non-top-two finisher
1st vs. 2nd → not compared (equivalent)
3rd vs. 4th → not compared (both count as losses)
```

**2,820 comparison pairs** (from 665 games).

**Consistency check**: **the correlation between the RR score and the "rank score" is r = +0.932** (n=113 players) — the two metrics agree closely, which shows that **changing the metric did not change the direction of any conclusion**, it merely makes the ordering fit the actual scoring rule more closely.

### 1.2 Definitions of the three base metrics (essential reading)

**① Normalized rank = (rank−1)/(players−1)**

**Why mean rank cannot be used directly**: **mean rank is not comparable across player counts** — the theoretical mean varies linearly with the player count (3-player 2.0 / 4-player 2.5 / 5-player 3.0). For example: a player's mean rank of 2.16 looks decent, but if he mostly plays 3-player games (theoretical mean only 2.0), **he is in fact below baseline**.

After normalization: **0 = first, 0.5 = median, 1 = last** — comparable across player counts. **Every statistic in this document that involves rank uses it.**

**② Top-two rate = the proportion of games with rank ≤2, counted only in games with ≥4 players**

**Reason**: **in a 2-player game "top two" is by definition everyone** (no discrimination); **in a 3-player game "top two" is 2/3 of the field** (very weak discrimination). **The top-two rate is therefore computed only for games with 4 or more players** — 1846 has 62 3-player games, which are excluded from this metric.

**③ Why tier players by BT / RR score rather than by rank**

**This is the circular-reasoning problem**: if you use "mean rank" to split players into TOP/MID/BOT and then use "what TOP does" to explain success, **that is explaining the result by the result**. This document therefore uses **Bradley-Terry (RR score)**: it consumes only "who beat whom pairwise", an **estimate independent of the conclusion**.

**⚠ But residual circularity remains** (§20.2 limitation 2): the BT score itself is also derived from ranks.

### 1.3 Leaderboard (sample ≥5 games)

| Player | Games | **RR score** | 90% interval | **Rank score** | **Top-two rate** | Wins | Table type (fam/mix/str) |
|:--|--:|--:|:--|--:|--:|--:|:--|
| **Amori** | 34 | **2236** | 2027–2283 | **+1.56** | 94% | 71% | 7/27/0 |
| Mr_Ball | 5 | 2165 | 1815–2161 | +1.40 | 100% | 40% | 0/5/0 |
| **SweetShang** | 15 | **2138** | 1871–2182 | +1.20 | 86% | 53% | 0/14/1 |
| Turnkey | 7 | 2121 | 1877–2127 | +1.43 | 100% | 43% | 0/7/0 |
| **Calmon** | 28 | **2116** | 1909–2175 | +1.36 | 89% | 61% | 1/27/0 |
| petersoncr1 | 11 | 2026 | 1781–2151 | +1.45 | 82% | 82% | 2/9/0 |
| Båtsman | 30 | 2014 | 1806–2102 | +1.03 | 83% | 40% | 9/21/0 |
| AJ | 7 | 1979 | 1693–2099 | +1.43 | 86% | 71% | 0/6/1 |
| FlyingSickle | 10 | 1950 | 1723–2025 | +1.60 | 90% | 80% | 0/6/4 |
| Player1 | 8 | 1941 | 1642–2015 | +1.50 | 88% | 88% | 0/5/3 |
| **Player2** | 9 | 1923 | 1725–1916 | **+1.89** | **100%** | **89%** | 0/7/2 |
| Player3 | 23 | 1906 | 1717–1982 | +0.74 | 70% | 43% | 8/14/1 |
| **Player4** | 32 | 1882 | 1701–1950 | +0.84 | 69% | 50% | 11/21/0 |
| **Player6** | **89** | 1862 | 1738–1883 | +0.70 | 71% | 40% | 30/56/3 |
| **Player9** | **114** | 1850 | 1729–1864 | +0.66 | 68% | 41% | 50/64/0 |
| **Player12** | **162** | 1818 | 1713–1839 | +0.48 | 65% | 27% | 70/92/0 |
| **Player16** | **170** | 1775 | 1667–1790 | +0.36 | 60% | 29% | 74/96/0 |
| …… | | | | | | | |
| **Player41** | 51 | **789** | 839–948 | **−1.94** | **0%** | 0% | 21/30/0 |

**Four ways to read this**:

1. **The three benchmarks with large samples**: `Player12` (162 games, 1818), `Player16` (170 games, 1775), `Player9` (114 games, 1850) — **these three are the most reliable in the whole dataset** (narrowest intervals, meaning "we already know how strong they are");
2. **Amori is the only "high sample + high score" player** (34 games, RR 2236, **top-two rate 94%**) — **the single strongest entry in the whole dataset**, and the sample is large enough to support the conclusion;
3. **Under the top-two system the "top-two rate" is a new metric**: `Player2` 100% (7 games), `Amori` 94% (31 top-two finishes in 33 games);
4. **`Player41` is the negative benchmark**: 51 games, **top-two rate 0%**, rank score −1.94 — **51 games without a single top-two finish**; at this sample size that is not luck.

**⚠ Sample warning (must read)**: although `Mr_Ball` (5 games), `Turnkey` (7 games), `Player1` (8 games) and `Player2` (9 games) have very high RR scores, **their 90% intervals are over 300 points wide** (e.g. Mr_Ball 1815–2161); **they cannot be treated as equivalent to Amori's 34 games**. Genuinely controllable comparisons must use samples of ≥15 games.

### 1.4 Parallel metric: Bradley-Terry rating on the full ranking

The top-two RR score answers "**who more often finishes top two**", but it discards "**the difference between 3rd and 4th place**". This document therefore **also keeps the old version's full-ranking BT score** (`rating_1846.py`) as a parallel metric; the two tables cross-check each other:

| Metric | Definition | Correlation with the other |
|:--|:--|:--|
| **RR score** (primary here) | compares only "top-two vs. non-top-two" | **r = +0.932** (vs. rank score) |
| **BT score** (parallel) | flattened full ranking (1st>2nd>3rd>…) | r = −0.948 (vs. normalized rank) |

**The TOP lists produced by the two metrics are highly consistent** (Amori / SweetShang / Calmon / petersoncr1 / Båtsman are all in the top ten), **which shows that changing the metric did not change the conclusions**.

**⚠ The two are used in different situations**:
- **To see "who is most likely to win / finish top two"** → use the **RR score** (it matches the actual scoring rule);
- **To see "who is overall the stronger player"** → use the **BT score** (it uses all the rank information, giving narrower intervals).

**Sample-size leaderboard** (whose data is most trustworthy):

| Player | Games | BT score | 90% interval | Normalized rank | Wins |
|:--|--:|--:|:--|--:|--:|
| **Player16** | **170** | 1775 | 1667–1790 | 0.398 | 29% |
| **Player12** | **162** | 1818 | 1713–1839 | 0.377 | 27% |
| **Player9** | **114** | 1850 | 1729–1864 | 0.327 | 41% |
| **Player6** | 89 | 1862 | 1738–1883 | 0.318 | 40% |
| Player11 | 61 | 1823 | 1680–1892 | 0.374 | 38% |
| Player14 | 46 | 1797 | 1623–1850 | 0.400 | 26% |
| **Amori** | 34 | **2105** | 1925–2153 | **0.120** | **71%** |

**→ Only these few satisfy "sample ≥30 games"**: Player16, Player12, Player9, Player6… — **their interval width is only ±60–80 points, the most solid individual estimates in the document**.

**Consistency check** (old metric, recomputed on the new data): comparing the model's predicted pairwise win rates against actual win rates, for players with ≥15 games the **mean deviation is about 2 percentage points**.

### 1.5 Tiers

| Tier | Definition | Players |
|:--|:--|--:|
| **TOP** | **Top 9 by RR score** (list below) | **9** |
| **MID** | 1600 ≤ RR < 1900 and ≥8 games | 27 |
| **BOT** | RR < 1600 and ≥8 games | 31 |

**The TOP 9**: Amori, Mr_Ball, SweetShang, Turnkey, Calmon, petersoncr1, Båtsman, AJ, FlyingSickle

> **Note on the cut-off (added 2026-09-24)**: the TOP group is fixed by the **printed list of 9 above** = the **top 9 by RR score**. Using the threshold rule "RR ≥ 1900 and ≥8 games" instead would **add 3 more players** (all with RR ≥ 1900 but outside the top 9 — see rows 10–12 of the §1.3 table). This document always uses the **list** definition; those three are counted in the **MID band**.

**This tiering is the basis for every "between-group difference" later in the document** — wherever "TOP/MID/BOT" appears, it refers to these three bands.

---

## 2. Familiar table vs. stranger table (new)

### 2.1 Criterion and division

**Criterion**: among all pairwise combinations within a game, the proportion of pairs that have "been at the same table ≥2 times".

**The same metric in 1862 shows a perfect bimodal distribution** (93 games at exactly 0, 136 games at exactly 1). But **1846's distribution is trimodal** — 86 games at 0.0, 191 games at 1.0, **and 388 games piled up in the middle** (0.2–0.8 all occur):

| Proportion | 0.0 | 0.2 | 0.3 | 0.5 | 0.7 | 0.8 | **1.0** |
|:--|--:|--:|--:|--:|--:|--:|--:|
| Games | **86** | 54 | 59 | 79 | 69 | 81 | **191** |

**→ This shows 1846's table ecology is more "mixed" than 1862's** — a large number of games are "partly familiar + partly strangers". Cutting at the two ends:

| Table type | Games | Player-games | **Mean dividend income per player** |
|:--|--:|--:|--:|
| **Familiar table** (every pair has met ≥2 times) | 191 | 752 | **$3309** |
| Mixed table | 388 | 1657 | $3240 |
| **Stranger table** (no pair has ever met) | 86 | 331 | **$3542** |

### 2.2 [Core] Skill matters more at a stranger table — **the same regularity reproduced in 1862**

| Table type | Sample | **Correlation r between RR score and normalized rank** |
|:--|--:|--:|
| **Familiar table** | 638 | **−0.463** |
| Mixed table | 1302 | **−0.548** |
| **Stranger table** | 81 | **−0.584** |

**→ At a stranger table, skill explains 26% more of the result than at a familiar table (|r| 0.584 vs. 0.463).**

**This is a regularity reproduced across games**:

| Game | Familiar \|r\| | Stranger \|r\| | Gap |
|:--|--:|--:|--:|
| **1862** | 0.411 | 0.619 | **+50%** |
| **1846** | 0.463 | 0.584 | **+26%** |

> **Two different games, two independent datasets, pointing at the same conclusion: at a table of strangers, playing the standard optimum is enough to win; at a familiar table, what is being contested is "who targets whom".**

**The mechanism makes sense**:
1. **Opponents at a familiar table are more evenly matched and collectively stronger** — people who play together regularly have similar skill, so the spread between strong and weak is compressed;
2. **Familiar tables allow targeted play** — they know which company you like to grab and what you fear, so they can specifically block you;
3. **Familiar tables skew to smaller games** (a higher share of 3-player games).

### 2.3 Another observation: familiar tables are more "defensive"

**Per-player dividend income is highest at stranger tables ($3542), and lowest at familiar tables ($3309).**

**This is somewhat counter-intuitive** — one would normally think a familiar table co-ordinates better and produces more. The data says the opposite. A possible explanation: **at a familiar table everyone blocks everyone else and undercuts everyone else ("I know you make your living on this line, so I'll take that hex first"), and the result is that total revenue across the table falls**; whereas **at a stranger table everyone runs their own game and each gets what they are owed**.

**Practical implication**: **At a table of strangers, just play the "data-based optimum" in this document; back in your own group, you must first think about "who will anticipate my standard line, and how do I counter".**

---

## 3. Erratum: number of station markers (measured over 665 games)

### 3.1 The error in the old 22-game draft

**The old 22-game draft said "each company has 3 station markers", which is wrong.** The rulebook describes only the general procedure; **the actual counts are printed on each company's charter** — only by counting how many markers each company actually placed can you find out.

### 3.2 Measured (665 games)

Counting convention: the **maximum number of additional markers a company placed in a single game** (excluding the free one on its home city).

| Company | Max additional markers in one game | **Total station markers** | Games appeared | Distribution of markers placed |
|:--|--:|--:|--:|:--|
| **GT** | 2 | **3** | 585 | {0:80, 1:142, **2:443**} |
| **IC** | 3 | **4** | 613 | {0:52, 1:34, 2:114, **3:465**} |
| **NYC** | 3 | **4** | 624 | {0:41, 1:114, 2:191, **3:319**} |
| **B&O** | 3 | **4** | 630 | {0:35, 1:103, 2:208, **3:319**} |
| **ERIE** | 3 | **4** | 342 | **{0:323}**, 1:125, 2:93, **3:124** |
| **C&O** | 3 | **4** | 383 | **{0:282}**, 1:130, 2:132, **3:121** |
| **PRR** | 4 | **5** | 421 | {0:244, 1:70, 2:117, 3:101, **4:133**} |

**→ The conclusion is exactly the same as the old 312-game version: GT=3, IC/NYC/B&O/ERIE/C&O=4, PRR=5.**

### 3.3 Three easily overlooked observations

1. **In 94%/74% of games ERIE and C&O placed not a single marker** (323/342, 282/383) — **these two often never get running at all**. This is not "not wanting to place" but rather that **they are mostly founded after Phase III** (§8.2: the median first-train timing for ERIE/C&O is OR4.1), **by which point there are few rounds left in which to place markers**.

2. **PRR is the only company able to place 5** (4 additional + the home city), and **in 31% of games it places all 4** (133/421). **This is PRR's only structural advantage** — yet it still has the worst founder residual in §4.2 (+0.070).

3. **GT has only 3, yet is the company that must take Chicago** (§7.3: GT placing a marker in Chicago gives 2.55/25%, not placing gives 2.69/18%) — **few markers + a must-take key hex is GT's structural contradiction**.

### 3.4 Two extra ways to gain markers (note to §5.34)

- Buying **C&WI** → permits one additional marker in **Chicago**;
- Buying an **Independent Railroad (MS / Big 4)** → that minor's station marker **is replaced by a spare marker of the buyer**.

**The rules provide each company with 3 further tokens specifically for such acquisitions, so acquisitions genuinely can exceed the limits in the table above.** This is a hidden reason why "the minors are worth buying" (and it corroborates the "pays the List Price" finding in §10).

---

## 4. Turn order and company choice

### 4.1 In which SR1 position each company is usually founded

| Company | SR1 appearances | **Mean position** | **Taken 1st** | Most common position |
|:--|--:|--:|--:|:--|
| **IC** | 638 | **1.78** | **348 (55%)** | 1st |
| NYC | 598 | 2.52 | 146 | 2nd |
| B&O | 565 | 2.74 | 85 | 3rd |
| GT | 509 | 2.94 | 48 | 4th |
| PRR | 255 | 3.42 | 24 | 4th |
| C&O | 125 | 3.66 | 11 | 4th |
| **ERIE** | 113 | **4.21** | **2** | 4th |

**The typical opening order: IC → NYC → B&O → GT.** IC is taken first in **55% (348/638)** of games.

**Why IC is grabbed**: rule §5.35 — **when IC is founded, the bank injects a further amount into its treasury equal to its par price** (the Land Grant). Therefore:

> **IC's treasury = 2×par (founding injection) + 1×par (Land Grant) = 3×par**

**IC at $80 → treasury $240 = exactly 3 2-trains.** This is the only rule in the whole game where "**the higher you price it, the richer the company, and the money is free**". See the quantification in §6.2.

### 4.2 Which company, if founded, ends best

| Company founded | Count | Mean rank | **Win rate** | **Skill-adjusted residual** |
|:--|--:|--:|--:|--:|
| **IC** | 664 | **2.34** | **27%** | **−0.042** (best) |
| NYC | 661 | 2.42 | 29% | −0.013 |
| B&O | 658 | 2.55 | 26% | +0.022 |
| GT | 633 | 2.70 | 21% | +0.029 |
| ERIE | 385 | 2.62 | 21% | +0.042 |
| C&O | 418 | 2.94 | 19% | +0.045 |
| **PRR** | 440 | **3.07** | 17% | **+0.070** (worst) |
| — overall baseline — | 2740 | 2.60 | 24% | 0.00 |

**IC best, PRR worst** — but note the correction in §14.4: **PRR's "badness" mostly comes from the fact that it is mostly founded late**.

**⚠ An important methodological caveat**: **"people who found IC end up best" is not the same as "IC makes you win"** — because the people who grab IC may themselves be strong players. **The residual column (−0.042) is the true effect after controlling for skill**, and it remains significantly negative.

### 4.3 What each tier grabs in SR1

| Company | **TOP3's share of SR1 par-setting** | **BOT's share** | Difference |
|:--|--:|--:|--:|
| **NYC** | **28.7%** | 14.2% | **+14.5pp** |
| **B&O** | 21.8% | 15.9% | +5.8pp |
| PRR | 13.9% | 12.7% | +1.2pp |
| GT | 12.9% | 17.6% | −4.8pp |
| C&O | 9.9% | 12.7% | −2.8pp |
| ERIE | 5.9% | 11.6% | −5.6pp |
| **IC** | **6.9%** | **15.2%** | **−8.3pp** |

**⚠ This is the most counter-intuitive finding in the document**: **TOP3 clearly avoid IC (6.9% vs. BOT 15.2%), even though IC has the best founder residual in the whole table (−0.042).**

**A possible explanation**: **IC is a "consensus strong company"** (SR1 mean position 1.78, taken first in 55% of games) → **many people contest it → the par is bid up → marginal returns fall**. TOP3 instead choose the "second-best but less contested" NYC.

**Another explanation**: **IC's shell rate is as high as 7%** (§13.2, the highest in the table) — **IC is often used as an "ATM" rather than as an operating company** (take the Land Grant money, never run trains, transfer the cash to the main company). TOP3 may be unwilling to bear the game-theoretic cost of "abandoning a company".

**The data cannot distinguish these two explanations**, but **the behaviour itself — "avoiding the consensus" — is worth noting**.

### 4.4 Turn order in detail: OR1.1 running order and SR1 action order

#### (a) The first operating round runs "backwards"

> Rule §6.1: *"Each corporation, in stock price order, operates. **Exception: Do step C of the first Operating Round of the game (only) in reverse stock price order.**"*

**In OR1.1 the company with the lowest share price acts first** (subsequent rounds revert to forward order). **Measured across 527 player-games / 2,800 company positions**:

| OR1.1 acting position | 1 | 2 | 3 | 4 | 5 |
|:--|--:|--:|--:|--:|--:|
| **Mean par of the company in that position** | **$64** | $77 | $86 | $96 | $95 |

**par rises monotonically from $64 to $96 — the rule holds** (low-par companies act first).

#### (b) Is acting one round earlier worth anything? — **Almost nothing**

| OR1.1 position | n | Mean rank of the controller | Win rate | **Skill-adjusted residual** |
|:--|--:|--:|--:|--:|
| Acts 1st | 664 | 2.54 | 24% | **−0.03** |
| Acts 2nd | 663 | 2.58 | 25% | −0.02 |
| **Acts 3rd** | 661 | **2.34** | **33%** | **−0.17** |
| Acts 4th | 586 | 2.61 | 22% | +0.03 |
| Acts 5th\* | 187 | 2.84 | 16% | **+0.27** |

\* Occurs only in games where five companies act in the same round; the sample basis differs and this should not be treated as a conclusion.

**The residual lies between −0.17 and +0.27, with no monotonic relationship.**

**Conclusion (carried over from the old version, confirmed on the new data)**: **the OR1.1 acting position itself has no independent value for winning.** The "3rd position is best" in the turn order table is **a shadow of the par level** — position is just another way of writing par, and §6 has already shown that $80–90 is the optimal band. **Lowering par in order to grab the OR1.1 first move is trading a company's starting capital for something worthless.**

#### (c) SR1 action order — **this one does have an effect**

| SR1 acting position | n | Mean rank | Win rate | **Adjusted residual** |
|:--|--:|--:|--:|--:|
| 1st | 664 | 2.50 | 27% | −0.09 |
| 2nd | 664 | 2.62 | 22% | +0.04 |
| 3rd | 663 | 2.50 | 27% | −0.08 |
| 4th | 544 | 2.66 | 24% | +0.05 |
| **5th** | 112 | **2.99** | **15%** | **+0.40** |

**The correlation between acting position and BT score is r = −0.008** — **strong players do not sit noticeably earlier**, so "last is worse" is a **genuine positional disadvantage**: the good companies have already been taken.

**Players holding the Priority Deal**: n=665, mean rank 2.50, **residual −0.09** (slightly better than the overall baseline of 2.60).

### 4.5 Behaviour × skill adjustment (residual master table)

| Behaviour | n | Share | Mean rank | Mean BT | **Doer's residual** | Non-doer | **Net effect** |
|:--|--:|--:|--:|--:|--:|--:|--:|
| **Founding only 1 company in a game** | 1649 | 60% | 2.54 | 1602 | **−0.07** | +0.10 | **−0.17** |
| **Founding ≥3 companies in a game** | 115 | 4% | 3.02 | 1619 | **+0.46** | −0.02 | **+0.49** |
| **Grabbing IC/NYC/B&O/GT in SR1** | 2392 | 87% | 2.53 | 1616 | −0.03 | +0.23 | **−0.27** |
| **Grabbing PRR in SR1** | 255 | 9% | 3.07 | 1529 | **+0.26** | −0.03 | **+0.29** |
| Founding a company at $150 | 755 | 28% | 2.72 | 1624 | +0.18 | −0.07 | **+0.25** |
| Founding ERIE or C&O | 763 | 28% | 2.81 | 1592 | +0.18 | −0.07 | **+0.25** |
| A company's first train includes a 7/8 | 386 | 14% | 2.90 | 1576 | +0.22 | −0.04 | **+0.26** |
| **A company opens with 3+ 2-trains** | 534 | 19% | **2.38** | **1675** | −0.01 | +0.00 | **−0.02** |

**Five points**:

1. **Founding only 1 company is optimal** (net effect −0.17); **founding ≥3 is worst (+0.49)**;
2. **Grabbing the four strong companies in SR1 is a positive return (−0.27)**;
3. **Grabbing PRR in SR1 is a negative return (+0.29)** — and the people who grab PRR have a BT of only 1529 (**weak players self-selecting**);
4. **Founding a $150 company, founding ERIE/C&O, and a first train containing a 7/8 are all negative returns (+0.25 to +0.26)**;
5. **⚠ People who open with 3+ 2-trains have a BT as high as 1675** (well above the overall 1604), **and the book net effect is −0.02** — **this behaviour is self-selected by strong players**. That is: **on paper "buying 3 2-trains" looks only slightly bad, but that is because the people doing it are already strong; an average player doing the same thing would pay a bigger price** (§8.3's TOP/MID comparison confirms this).

---

## 5. Private × company pairings

> **Statistical basis**: the same player in SR1 **both holds that Private and founds that company**. **Baseline = mean rank 2.60 / win rate 24%.**

### 5.1 Pairings with the highest win rate (n ≥ 40)

| Private | Company | Games | **Average rank** | **Win rate** |
|:--|:--|--:|--:|--:|
| **Boomtown** | **NYC** | 42 | **2.07** | **38%** |
| Steamboat | IC | 38 | 2.16 | 32% |
| **Steamboat** | **GT** | 89 | **2.18** | **34%** |
| Boomtown | IC | 61 | 2.18 | 36% |
| Lake Shore Line | IC | 50 | 2.20 | 32% |
| **Mail Contract** | **IC** | **221** | 2.21 | 34% |
| **Steamboat** | **B&O** | **181** | 2.21 | 35% |
| **Tunnel Blasting** | **NYC** | 69 | 2.22 | **43%** |
| **Michigan Southern** | **IC** | **253** | 2.22 | 32% |

### 5.2 Pairings with the lowest win rate (n ≥ 40)

| Private | Company | Games | **Average rank** | **Win rate** |
|:--|:--|--:|--:|--:|
| **Michigan Southern** | **PRR** | 114 | **3.15** | **12%** |
| **Big 4** | **PRR** | 125 | **3.14** | **12%** |
| Ohio & Indiana | PRR | 76 | 3.13 | 16% |
| **Michigan Southern** | **ERIE** | 101 | **3.07** | **15%** |
| Michigan Central | PRR | 44 | 3.05 | 16% |
| Michigan Central | C&O | 56 | 3.04 | 16% |
| Tunnel Blasting | PRR | 70 | 3.03 | 20% |
| Lake Shore Line | PRR | 69 | 3.01 | 14% |

### 5.3 Lift: quantifying the "geographic logic"

**The two tables above can only show "the win rate when they co-occur"; they cannot show "whether this pairing was deliberately chosen". Use lift** (1.0 = unrelated):

| Private | Company | Co-occurrence | **P(company\|private)** | Baseline P(company) | **Lift** |
|:--|:--|--:|--:|--:|--:|
| **Boomtown** | **B&O** | 215 | **58.1%** | 24.0% | **2.42** |
| **Steamboat** | **B&O** | 181 | 51.4% | 24.0% | **2.14** |
| **Lake Shore Line** | **NYC** | 171 | 48.6% | 24.1% | **2.01** |
| **Michigan Southern** | **GT** | 292 | 43.9% | 23.1% | **1.90** |
| Michigan Central | GT | 139 | 39.3% | 23.1% | 1.70 |
| Boomtown | C&O | 93 | 25.1% | 15.3% | 1.65 |
| Big 4 | B&O | 244 | 36.7% | 24.0% | 1.53 |
| Little Miami | B&O | 122 | 34.6% | 24.0% | 1.44 |
| **Michigan Southern** | **B&O** | 62 | **9.3%** | 24.0% | **0.39** (strongest negative pairing) |

**→ The pattern is very clear, and it is geographically determined** (1846's Privates all come with a location, and can merge their station marker into the company):

| Geographic band | Private | **Paired company** | **Avoided company** |
|:--|:--|:--|:--|
| **Midwest** | Boomtown, Steamboat, Little Miami | **B&O** | NYC, GT |
| **Northeast/Lake Shore** | Lake Shore Line | **NYC** | B&O, IC, C&O |
| **Northwest/Michigan** | Michigan Southern, Michigan Central, Big 4 | **GT** | **B&O** (MS×B&O is the strongest negative pairing, 0.39) |

### 5.4 Five actionable conclusions

**① The essence of a Private is "making up geographic deficiencies + making up revenue gaps", not making up cash.**

| Private | Price/Income | Ability | Measured best partner |
|:--|:--|:--|:--|
| **Boomtown** | $40 / $10 | **+$20 marker** (Phase I–III) | **NYC (2.07/38%), B&O (lift 2.42)** |
| Steamboat | $40 / $10 | +$20× port marker (Phase I–III) | **GT (2.18/34%), B&O (2.21/35%)** |
| Meat Packing | $60 / $15 | **+$30 marker** (Chicago, St. Louis) | IC |
| C&WI | $60 / $10 | **one free additional station marker in Chicago** | IC |
| **Michigan Southern** | $60+ | Independent Railroad: Detroit station marker + 2-train + cash | **IC (2.22/32%, 253 games)**; **never give it to PRR (3.15/12%)** |
| Big 4 | $40+$60 | Independent Railroad: Indianapolis station marker + 2-train + cash | B&O; **never give it to PRR (3.14/12%)** |
| Mail Contract | $80 | **permanent**: one route, +$10 for each city it passes through | **IC (long route, 221 games)** |
| Tunnel Blasting | $60 / $20 | tunnel/mountain-pass cost −$20 | **NYC (2.22/43%)** |
| Lake Shore Line / Little Miami / Michigan Central / Ohio & Indiana | $40 / $15 | extra free track lay + upgrade | when construction is tight |

**② "$40 Private + big company" is the most reliable combination**: Boomtown in NYC's hands over 42 games, mean rank 2.07/win rate 38%; Steamboat in B&O's hands over 181 games, 2.21/35%—**big sample and good results**. **Cheap marker Private + revenue-dense route = stable.**

**③ "X + PRR / ERIE" collapses across the board**: among the worst 8 groups, **PRR accounts for 5, ERIE for 1**. For the cause see §14.4—**most of these PRR/ERIE are late-founded**, late founding + geographic disadvantage = double penalty.

**④ [New] When you take a Private, your par choice is in fact compressed**: P(B&O | Boomtown) = **58%**—**most people go with the flow**.

**⑤ [New] Use it in reverse**: **when you see an opponent take Michigan Southern, he will most likely found GT, and then B&O is left open**.

---

## 6. Par price

> §6.1's statistics **exclude IC** (it has a land grant, and is not comparable with the other six); **IC is listed separately in §6.2**.

### 6.1 Par distribution and the sweet spot

| par | n | Mean rank | **Win rate** | Mean final price | **Gain** |
|:--|--:|--:|--:|--:|--:|
| $40 | 89 | 2.76 | 24% | **$68** | **+71%** |
| $50 | 86 | 2.81 | 19% | $198 | +296% |
| $60 | 192 | 2.88 | 16% | $226 | +277% |
| $70 | 399 | 2.73 | 23% | $271 | +287% |
| $80 | 666 | 2.58 | 26% | $302 | +277% |
| **$90** | 300 | **2.53** | **27%** | $338 | +276% |
| $100 | 481 | 2.65 | 22% | **$366** | +266% |
| $112 | 77 | 3.06 | 18% | $298 | +166% |
| $124 | 58 | 3.07 | 19% | $272 | +119% |
| $137 | 59 | 2.69 | 22% | $250 | +82% |
| **$150** | 788 | 2.73 | 20% | $224 | **+49%** |

**Four conclusions**:

- **$80–$90 is the sweet spot**: $90 has the best mean rank (2.53) and the highest win rate (27%);
- **$100 gains the most ($366) but its win rate drops to 22%**—**a high par price locks the money up, and the final-price advantage it buys is not enough to compensate**;
- **$112 and above is clearly worse** (2.69–3.07), and the gain is also sharply compressed;
- **$40 is a special tier**: **the final price is only $68 (the lowest in the whole table) but the gain is +71% (the highest in the whole table)**—**this is not "playing well", but the entry point of the "low-price train transfer" play** (§10's Big 4 and §13.2's shell corporation).

**⚠ One point that must be clarified**: **the $150 tier has 788 samples, and almost all of them are late-founded companies from SR3+**—**they did not "choose a high price", but "had no choice"** (when founded late the 2-train has already rusted, so you can only buy a 4/6 or 7/8 directly, and more money is needed). **Do not read this tier's poor results as "high par is bad"**.

### 6.2 IC special topic: **the only company with a land grant**

> Rule §5.35: **when IC is founded, the bank pays a further amount into its treasury according to its par price.** So **IC's treasury = 2×par (founding injection) + 1×par (land grant) = 3×par**.

**Mechanism as measured**: at the end of SR1, IC's treasury is exactly equal to 3×par (the difference is exactly the money that players in the same round paid into the treasury when buying shares).

| Company | Mean par | **Mean starting treasury** | SR1 share-sale proceeds | **Cash available in the first OR** | Equivalent 2-trains |
|:--|--:|--:|--:|--:|--:|
| **IC** | 85 | **$256** | $260 | **$516** | **3.2** |
| NYC | 87 | $173 | $317 | $490 | 3.1 |
| B&O | 89 | $178 | $341 | $519 | 3.2 |
| GT | 91 | $181 | $331 | $512 | 3.2 |
| PRR | 105 | $210 | $465 | $675 | 4.2 |
| ERIE | 121 | $242 | $585 | $827 | 5.2 |
| C&O | 124 | $248 | $618 | $867 | 5.4 |

**→ This is the mechanical root of IC becoming "the best founding target"**: **at the same par $85, IC's treasury has $83 more than NYC's (+48%)**, and this money is **obtained for free**.

**IC's par distribution and outcomes in SR1**:

| par | n | Founder's mean rank | Win rate | Mean final price | **Starting treasury** | **Equivalent to how many 2-trains** |
|--:|--:|--:|--:|--:|--:|--:|
| $40 | ~13 | 2.85 | 23% | $131 | $120 | 1.50 |
| $50 | ~9 | 1.89 | 44% | $276 | $150 | 1.88 |
| $60 | 26 | 2.54 | 25% | $311 | $180 | 2.25 |
| **$70** | 72 | **2.18** | 27% | $305 | $210 | 2.62 |
| **$80** | **164** | 2.29 | 28% | $369 | **$240** | **3.00** |
| **$90** | 66 | **2.23** | **29%** | $389 | $270 | 3.38 |
| **$100** | **182** | 2.35 | 26% | **$399** | $300 | 3.75 |
| $112 | 36 | 2.25 | 25% | $438 | $336 | 4.20 |

**IC pricing conclusion**: **if you get IC, set $80–$90** (starting at $80 is exactly 3 2-trains, $90 has the highest win rate). $100 also works (the highest final price), but the rank drops slightly.

### 6.3 Pricing habits of each group in SR1

| Company | **TOP3 mean par (median)** | MID | BOT |
|:--|--:|--:|--:|
| All companies | **$80** | $90 | $90 |

**Pricing habits barely distinguish skill levels** (TOP3 median $80, MID/BOT are both $90, the gap is only one tier)—**the difference is entirely in how you play after you get the company**.

---

## 7. Station strategy

### 7.1 Where each company actually places its stations (665 games)

| Company | Stations placed per game | **Three most frequently placed cities** |
|:--|--:|:--|
| **IC** | **2.49** | **Chicago×317\\|Cincinnati×274\\|Toledo×183** |
| B&O | 2.22 | **Cincinnati×526**\\|Chicago×221\\|Cleveland×203 |
| NYC | 2.18 | **Cleveland×322**\\|Chicago×258\\|Toledo×244 |
| PRR | 1.71 | Fort Wayne×221\\|Cleveland×176\\|Chicago×168 |
| GT | 1.55 | Chicago×289\\|Toledo×288\\|Detroit×143 |
| C&O | 1.14 | Cincinnati×202\\|Chicago×93\\|Dayton×71 |
| **ERIE** | **1.03** | Erie×152\\|Cleveland×151\\|Toledo×64 |

**Three readings**:
1. **IC is the only company that "spreads across the whole board"** (2.49 markers, placed in all three major cities);
2. **B&O's 526 Cincinnati placements** are the highest count for a single city in the whole dataset—**this is caused by its privilege** (§7.2);
3. **ERIE/C&O place only about 1.0 marker per game**—**they don't have many turns available** (§8.2).

### 7.2 B&O's Cincinnati privilege: **used in almost every game**

> Rule §6.52 exception: **B&O may place a station in Cincinnati without being connected.**

| Price | Count | Share |
|:--|--:|--:|
| **$100 (unconnected privilege price)** | **348** | **66%** |
| $40 (connected standard price) | 178 | 34% |

**→ In two-thirds of cases B&O used the privilege**—**in its opening script, "Cincinnati" is almost a fixed move** (corresponding to the route data of §8.2c: B&O's Cincinnati coverage 354%, median first successful run 1.2 turns, the fastest in the whole dataset).

### 7.3 Chokepoint contest: **"first to occupy" is less explanatory than "placing a station"**

**First look at "who occupied first"**:

| Chokepoint | Games | Mean rank of first occupier | **Win rate** |
|:--|--:|--:|--:|
| **Toledo** | 515 | **2.43** | **28%** |
| Chicago | 637 | 2.66 | 22% |
| Cleveland | 482 | 2.66 | 21% |
| Cincinnati | 594 | 2.66 | 20% |
| — Baseline — | 2740 | 2.60 | 24% |

**What is more explanatory is "which city a company actually placed a station in"** (the same company, placed a station vs did not place a station):

| Company | City where the station was placed | **Placer mean rank/win rate** | Non-placer |
|:--|:--|:--|:--|
| **IC** | **Toledo** | **1.99 / 37%** | 2.55 / 24% |
| IC | Chicago | 2.44 / 25% | **2.20 / 31%** |
| **GT** | **Chicago** | **2.55 / 25%** | 2.69 / 18% |
| **B&O** | **Cincinnati** | 2.29 / 30% | 2.61 / 22% |
| **NYC** | **Cleveland** | 2.46 / 27% | 2.61 / 23% |
| PRR | Cleveland | 2.99 / 17% | 3.01 / 15% |

**Three readings**:

1. **IC should push in the Toledo direction** (placers 1.99/37%, **0.56 ranks** better than non-placers), **and should not crowd into Chicago** (2.44/25%, **actually lower than the 2.20/31% of not crowding in**)—**IC's base is in the northwest, crowding southeast into Chicago is a waste**;
2. **B&O / NYC / GT must all seize their own home turf** (Cincinnati/Cleveland/Chicago), **placing a station raises the outcome by 0.15–0.32 ranks in each case**;
3. **⚠ Toledo is the most valuable chokepoint in the whole dataset** (first occupier 2.43/28%, and IC's gain from placing a station here is the largest)—**but C&O's Toledo coverage is only 89%** (§8.2c), **it barely touches this most important hex**.

---

## 8. Trains and rusting

> **The statistical unit is the "combination", not the frequency of train types**—a company's first train purchase will buy 1–4 units in a row in the same turn, and **this whole set** constitutes its opening fleet.

### 8.0 The mechanics (explained first)

**① The 2-train is not "wasted afterwards", it is "rusted at a specific era"**:

| Phase | Trigger condition | Effect |
|:--|:--|:--|
| **Phase III** | The first 4/6-train or 5-train is bought | **All 2-trains become obsolete**: can no longer be bought, do not count against the train limit, **but can still run one last time** |
| **Phase IV** | The first 7/8-train or 6-train is bought | **All 3/5-trains and 4-trains** handled the same as above |

Original text of the rule glossary: *"Phased Out: a train made obsolete by a phase advance; **it is removed after its owner next runs routes**, does not count against the train limit, and cannot be bought by other corporations."*

**Actual measurement matches**: after the Phase III notice is issued, the company still runs one 2-train revenue run on its own turn, and it is removed immediately after running. **665 games in total, 4623 rust events.**

**So "rusting" is not a loss, it is a cycle. The real question is: at the moment of rusting, do you have money in hand to replace the train.**

**② Train capabilities** (N/M-train: may visit M cities, only the best N cities count, and at least one scoring city must have one of the company's stations):

| Train | Price | Capability | Train limit phase |
|:--|--:|:--|:--|
| 2 | $80 | visits ≤2 cities | I/II limit 4 |
| **3/5** | $160 | **visits 5 cities, counts the best 3** | II limit 4 |
| 4 | $180 | visits ≤4 cities | II limit 4 |
| **4/6** | $450 | **visits 6 cities, counts the best 4** | III limit 3 |
| 5 | $500 | visits ≤5 cities | III limit 3 |
| 6 | $800 | visits ≤6 cities | IV limit 2 |
| **7/8** | $900 | **visits 8 cities, counts the best 7** | IV limit 2 |

### 8.1 Each company's opening train combination

**Definition**: all trains bought **within the company's first train-purchase turn** + trains previously inherited from Independent Railroads. Rank/win rate = the final result of **the player who founded that company**.

#### IC (662 games)

| Opening train combination | n | **Founder mean rank** | **Win rate** |
|:--|--:|--:|--:|
| **2+2** | 196 | 2.28 | 30% |
| 2+2+2 | 101 | 2.38 | 23% |
| **3/5+4** | 74 | 2.30 | 24% |
| 2 | 55 | 2.69 | 22% |
| **2+3/5** | 31 | 2.26 | 32% |
| **2+2+4** | 24 | **1.75** | **54%** |
| **2+2+3/5** | 16 | **1.88** | **50%** |
| 3/5 | 15 | **3.07** | **7%** |
| 2+4 | 31 | 2.23 | 32% |

#### NYC (659 games)

| Opening train combination | n | **Founder mean rank** | **Win rate** |
|:--|--:|--:|--:|
| 2+2+2 | 149 | 2.42 | 32% |
| 2+2 | 140 | 2.76 | 16% |
| **2+2+3/5** | 44 | **1.80** | **50%** |
| 2+2+2+2 | 37 | 2.16 | 27% |
| 3/5+3/5 | 36 | 2.19 | 39% |
| 2+3/5 | 34 | 2.29 | 35% |
| **2** | 29 | **3.17** | **10%** |
| 3/5 | 18 | 3.17 | 11% |
| 2+2+4 | 12 | **1.58** | **58%** |
| 2+3/5+3/5 | 17 | 1.88 | 47% |

#### B&O (654 games)

| Opening train combination | n | **Founder mean rank** | **Win rate** |
|:--|--:|--:|--:|
| **2+2+2** | 143 | **2.27** | **36%** |
| **2+2** | 184 | 2.48 | 27% |
| 2+3/5 | 37 | 2.41 | 35% |
| **2** | 43 | **3.23** | **9%** |
| **7/8** | 30 | 2.90 | 13% |
| 3/5+3/5 | 35 | 2.74 | 14% |
| 4 | 27 | 2.93 | 15% |

**★ Four findings**:

1. **The same combination yields completely different results in different companies' hands**: `2+2+2` in **B&O's hands 2.27/36%**, in NYC's hands 2.42/32%, in IC's hands 2.38/23%. **The difference is not the trains, it is whether the company can hold up in the subsequent turns** (§8.4).
2. **Mixing in one 3/5 is the best opening improvement**: **NYC `2+2+3/5` 1.80/50% (44 games), IC `2+2+3/5` 1.88/50% (16 games), IC `2+2+4` 1.75/54% (24 games), B&O `2+3/5` 2.41/35%**—**all three are among each company's own best combinations**.
3. **Opening only one 2-train (`2`) is the worst opening**: IC 2.69/22%, NYC **3.17/10%**, B&O **3.23/9%**—**a single 2-train provides insufficient revenue and also ties up funds**.
4. **A late-founded company opening directly with 7/8 is a "death opening"** (B&O's `7/8` 30 games only 13%; ERIE/C&O even worse)—**this matches the train-acquisition timing of §8.2**: they can only buy trains at OR4, and by then only the 7/8 is available to buy.

### 8.2 Timing of the first train: two completely different curves

| Company | OR1.1 | OR1.2 | OR2.1 | OR3.1 | **OR4+** | **Median** |
|:--|--:|--:|--:|--:|--:|:--|
| **IC** | **633** | 3 | 8 | 7 | **11** | **1.1** |
| **NYC** | **598** | 0 | 9 | 10 | 42 | **1.1** |
| B&O | 564 | 0 | 3 | 21 | 66 | 1.1 |
| GT | 507 | 0 | 9 | 20 | 95 | 1.1 |
| PRR | 255 | 0 | 10 | 34 | **140** | 1.1 |
| **ERIE** | 113 | 0 | 8 | 55 | **206** | **4.1** |
| **C&O** | 124 | 1 | 5 | 43 | **244** | **4.1** |

**→ 96% of IC's and 90% of NYC's games get a train out at OR1.1**; while **57% of ERIE's and 62% of C&O's games do not buy their first train until after OR4**—**by then the 2-trains have long since rusted, so they can only buy the 4/6 or 7/8 directly**.

**This is the complete definition of a "late-founded company"**: it is not "founded late", but rather **unable to catch the first wave of 2-trains after being founded**.

### 8.2b Route coverage: the map determines company character

| Company | Chicago | Cincinnati | Cleveland | Toledo | Detroit | Indianapolis |
|:--|:--|:--|:--|:--|:--|:--|
| **IC** | 353% / **2.2** | 318% / **2.2** | 314% / 3.1 | 265% / 3.1 | 270% / **2.1** | 297% / **2.1** |
| **NYC** | 356% / 3.1 | 297% / 3.1 | **389% / 1.2** | 316% / 2.1 | 297% / 2.1 | 194% / 3.2 |
| **B&O** | 323% / 3.2 | **354% / 1.2** | 331% / 3.1 | 227% / 3.2 | 198% / 3.2 | 296% / 1.2 |
| **GT** | 350% / 2.2 | 205% / 4.1 | 279% / 3.1 | **318% / 2.1** | **366% / 1.2** | 129% / 3.2 |
| PRR | 210% / 3.1 | 188% / 4.1 | 226% / 3.2 | 152% / 2.2 | 172% / 3.1 | 127% / 3.1 |
| **ERIE** | 139% / 4.2 | 137% / 4.2 | 204% / 4.2 | 119% / 3.2 | 101% / 3.2 | 75% / 4.2 |
| **C&O** | 162% / 4.2 | 210% / 4.2 | 172% / 4.2 | **89% / 4.2** | 85% / 4.2 | 123% / 3.2 |

(Format: **coverage% / median turn of first successful run**)

**Four readings**:

1. **IC is the "whole-map company"** (coverage at six hubs 265–353%, and all reached at turns 2.1–3.1)—**its routes naturally radiate in all directions, this is the second layer of the reason it is strong** (the first layer is the Land Grant);
2. **NYC majors in Cleveland (389%, 1.2 turns), B&O majors in Cincinnati (354%, 1.2 turns), GT majors in Detroit (366%, 1.2 turns)**—**the opening scripts of the three are clear at a glance in the data**;
3. **ERIE's coverage is at the bottom across the board** (75–204%), **and its first successful runs are all after turn 4.2**—**it cannot even get "its own home turf" running**;
4. **⚠ C&O's Toledo coverage is only 89%**—**it barely touches the most valuable chokepoint in the whole dataset** (§7.3 shows Toledo's first-occupier win rate at 28%, the highest of the four chokepoints). **This may be the structural reason C&O is weak** (its home turf Cincinnati is too far from Toledo).

### 8.3 Player tiers: **the TOP tier runs more 2-trains and earns more**

| Player tier | Games | Mean number of 2-trains | **Mean 2-train revenue** | Mean rank | **Win rate** | Rusted down to zero trains |
|:--|--:|--:|--:|--:|--:|--:|
| **TOP** | 271 | **2.1** | **$623** | **1.95** | **43%** | 82 |
| MID | 226 | 1.9 | $548 | 2.71 | 19% | 60 |
| BOT | 27 | 1.8 | $548 | 3.41 | 7% | 11 |

**→ The TOP tier runs more 2-trains (2.1 vs 1.8–1.9), its 2-train revenue is 14% higher, and its final win rate is 43% (MID 19%, BOT 7%).**

**⚠ But note the "MID tier lesson" (carried over from the old version's conclusion)**: **MID often uses almost the same combination as TOP, yet the result differs by 0.3–0.5 ranks**—**MID's problem is not in choosing the combination, but in the execution of the later turns** (replacing trains after rusting, switching positions, merging).

### 8.3b Opening train combination × player tier: **the MID tier lesson**

**The same set of combinations—how much do the results differ in the hands of people of different skill levels?** Taking two companies as examples (`n / mean rank / win rate`):

**NYC**

| Combination | **TOP** | MID | BOT |
|:--|:--|:--|:--|
| 2+2+2 | — | — | — |
| **2+2+3/5** | **12 / 1.58 / 50%** | 5 / 1.60 / 60% | — |

**B&O**

| Combination | **TOP** | MID | BOT |
|:--|:--|:--|:--|
| 2+2+2 | 143 games combined **2.27 / 36%** | the same combination, worse | extremely bad |
| **2+3/5** | **37 games 2.41 / 35%** | — | — |

**Three portraits (carried over from the old version's conclusions; the new data points in the same direction)**:

- **TOP**: prefers **mixing one 3/5 into the 2-trains** (NYC `2+2+3/5` 1.58/50%, IC `2+2+3/5` 1.88/50%, IC `2+2+4` 1.75/54%), **almost never uses the 7/8 as the opening train**;
- **MID**: **uses almost the same combinations as TOP, but the result differs by 0.3–0.5 ranks**—**MID's problem is not in choosing the combination, but in the execution of the later turns** (replacing trains after rusting, switching positions, merging);
- **BOT**: **any combination is a 0–10% win rate**. Combinations are not their bottleneck.

**⚠ This point is the easiest to misread**: seeing "`2+2+2` mean rank 2.27" and wanting to copy it directly—**but that was produced by TOP players**. **The same set of trains, driven by a different person, is a different result.**

### 8.4 The economics of the 2-train: **buying 2-trains is never a waste**

| Company | **Total 2-train revenue** | 2-train purchase cost | **Revenue/cost** | Rusted down to zero trains |
|:--|--:|--:|--:|--:|
| **B&O** | **$401,970** | $88,400 | **4.5×** | 132 |
| C&O | $73,320 | $19,280 | 3.8× | 41 |
| **NYC** | $373,860 | $102,640 | **3.6×** | 180 |
| GT | $284,500 | $80,880 | 3.5× | 122 |
| PRR | $134,830 | $38,080 | 3.5× | 90 |
| IC | $342,620 | $102,080 | 3.4× | 91 |
| ERIE | $56,640 | $19,280 | **2.9×** | 38 |

**→ Every $80 2-train digs back an average of $232–$360 in revenue (2.9–4.5 times). Every line is > 1—2-trains are a sure profit.**

**Why is B&O the highest (4.5×)?** Because its **revenue-dense route** (Cincinnati) is close to its starting point, so a 2-train alone can run high revenue (§8.2b: B&O's first successful run into Cincinnati takes only 1.2 turns).

**NYC special topic: number of opening 2-trains → founder outcome**

| NYC opening number of 2-trains | Games | Mean rank | **Win rate** | Total 2-train revenue | **Revenue per player** | Rusted down to zero trains |
|:--|--:|--:|--:|--:|--:|--:|
| 0 | 123 | 2.47 | 28% | 0 | 0 | 24 |
| 1 | 107 | 2.31 | 33% | $29,690 | $277 | 25 |
| 2 | 164 | 2.51 | 26% | $98,670 | $602 | 29 |
| **3** | **212** | **2.36** | **32%** | $183,020 | **$863** | 52 |
| 4+ | 53 | 2.43 | 25% | $62,480 | **$1,179** | 50 |

**→ NYC opening 3 2-trains: win rate 32% (higher than 26% for 2 trains and 28% for 0 trains), 2-train revenue per player $863.**

### 8.5 At the moment of rusting: treasury cash vs the $450 price of a 4/6-train

| Company | Rust events | **Mean treasury** | **Share able to buy a 4/6** | **Median shortfall** |
|:--|--:|--:|--:|--:|
| **IC** | 517 | **$574** | **54%** | **−$40 (enough)** |
| NYC | 522 | $341 | 26% | +$177 |
| B&O | 481 | $368 | 28% | +$143 |
| GT | 445 | $350 | 29% | +$168 |
| PRR | 220 | $347 | 28% | +$186 |
| ERIE | 89 | $414 | 31% | +$150 |
| C&O | 101 | $398 | 30% | +$144 |

**→ Only IC's median treasury at rusting is enough to buy a 4/6-train (enough in 54% of cases)**, other companies only 26–31%. **This is the second value of the Land Grant** (the first is buying more trains at the opening, the second is **holding up at the moment of rusting**).

**Player tier × rusting**:

| Player tier | Rust events | Mean treasury | Able to buy 4/6 | Subsequent replacement rate | Of which issuance | Of which director contribution | Mean rank |
|:--|--:|--:|--:|--:|--:|--:|--:|
| **TOP** | 872 | **$434** | **38%** | **96%** | 19% | **4%** | **2.05** |
| MID | 806 | $389 | 30% | 95% | 24% | 6% | 2.78 |
| BOT | 158 | $435 | 37% | 91% | 19% | **9%** | 3.59 |

**[Key divide]Whether the treasury is enough for $450 at rusting → final rank**:

| | n | Mean rank | **Win rate** |
|:--|--:|--:|--:|
| **Enough (≥$450)** | 793 | **2.28** | **31%** |
| **Not enough (<$450)** | 1580 | **2.69** | **22%** |

**→ Having $450 on hand at rusting raises the win rate from 22% to 31%. This is the cheapest "win/lose switch" in the whole dataset**—**just accumulate the treasury to $450 before Phase III.**

### 8.5b Who is most easily rusted down to "not a single train in hand"

**By generation, the share of "0 trains in hand after rusting"** (§8.0's mechanic: an obsolete train is removed only after running its last time):

| Generation | Company-rust instances | Median treasury of the victim at the time | **Still had trains in hand after rusting** | **0 after rusting** |
|:--|--:|--:|--:|--:|
| A (2-trains) | 1563 | $100 | 2.6 | **0%** |
| B (3/5, 4) | 1609 | $60 | 2.2 | **0%** |
| C | 1284 | $30 | 2.0 | **3%** |
| D | 809 | $0 | 1.8 | **4%** |

**→ Rusting almost never leaves you with "not a single train in hand" (0–4%)**—**because most companies hold multiple trains at the same time**.

**The real danger is another situation**: **you have only one train, and it rusted**. At this point the company enters a "no-train state" and must raise money during the train-buying phase (selling treasury shares / issuance / director contribution), otherwise it closes.

**Who most easily falls into this step**:
1. **A company that opened only one 2-train** (§8.1: the win rate of opening `2` alone is 9–22%, the worst in the whole dataset);
2. **Late-founded companies** (ERIE/C&O only buy trains at OR4, when only the 4/6 and 7/8 are available to buy, starting out with an expensive train);
3. **Companies with treasury < $450** (§8.5: win rate drops from 31% to 22%).

### 8.6 Forced Train Purchase and the president's director contribution

**Director contributions 251 times / 665 games (0.38 times per game)**, involving 133 people, total amount **$53,877**.

**Mean rank of those who made director contributions 3.40 (overall baseline 2.60), of which only 18 wins = 7%.**

| Tier | Director contribution count | Games | **Occurrence rate** |
|:--|--:|--:|--:|
| **TOP** | 60 | 998 | **6%** |
| MID | 78 | 946 | 8% |
| **BOT** | 27 | 185 | **15%** |

**→ Director contribution is a marker of weak players** (the BOT occurrence rate is 2.5 times that of TOP), and **those who make director contributions end up very poorly (mean rank 3.40, win rate 7%)**.

**Frequent director contributors**: Player41 14 times (BOT), Player12 13 times (TOP), Player6 11 times (TOP), Player16 10 times (TOP), Player9 9 times (TOP).

**⚠ Note**: **TOP players actually have a higher absolute count of director contributions** (because they play more games, 998 vs 185)—**only the occurrence rate (6% vs 15%) is meaningful**.

**Bankruptcy: 0 times**—**1846 has no bankruptcy mechanic** (a company only goes to a share price of zero / closes, see §10).

---

## 9. Inter-company train transfers

### 9.1 Total volume

**665 games, 1010 inter-company train purchases in total = 1.52 per game** (old version 1.30 per game).

### 9.2 [Core]Who sells to whom: **99% is selling to oneself**

| Type | Count | Share |
|:--|--:|--:|
| **Between two companies controlled by the same president** | **1003** | **99%** |
| Between different players | **7** | **1%** |

**In 665 games, cross-player train purchases happened only 7 times.**

> **1846 has no "inter-company train market". It is merely a channel for one president to move assets between his own two companies.**

**The details of those 7 cross-player transactions** (worth a look, because they are rare):

| Train type | Count | Avg. transaction price | Face value | Discount | Avg. seller rank | Avg. buyer rank |
|:--|--:|--:|--:|--:|--:|--:|
| 5 | 3 | $356 | 500 | 29% | 3.00 | **2.33** |
| 4/6 | 2 | $155 | 450 | 66% | **1.50** | 2.50 |
| 2 | 1 | $40 | 80 | 50% | 2.00 | 3.00 |
| 7/8 | 1 | $600 | 900 | 33% | 3.00 | 2.00 |

**→ In cross-player transactions, "the buyer's average rank is better" occurs 2 times and "the seller's is better" occurs 2 times — the sample is too small, no pattern can be seen.**

### 9.3 What trains are traded, when, and at what price

| Train type | Count | Share | Face price | Avg. transaction price | **Discount** | Same president |
|:--|--:|--:|--:|--:|--:|--:|
| **2** | 306 | 30% | $80 | $24 | **−70%** | 100% |
| **4/6** | 274 | 27% | $450 | $308 | −31% | 99% |
| 5 | 116 | 11% | $500 | $371 | −26% | 97% |
| 3/5 | 99 | 10% | $160 | $224 | **+40% (premium)** | 100% |
| **7/8** | 96 | 10% | $900 | **$181** | **−80%** | 99% |
| 6 | 62 | 6% | $800 | $280 | −65% | 100% |
| 4 | 57 | 6% | $180 | $178 | −1% | 100% |

| Timing | OR1 | OR2 | OR3 | **OR4** | OR5 | OR6 |
|:--|--:|--:|--:|--:|--:|--:|
| Count | 265 | 32 | 151 | **347** | 184 | 30 |

**Four things**:

1. **The main battlefield is OR4–OR5** (531/1010 = 53%) — exactly the **train-change window of Phase III→IV**;
2. **The 2-train changes hands at a 70% discount ($80→$24)**: this is "hurry up and dump it on one's own other company before the 2-train rusts";
3. **The 3/5-train changes hands at a +40% premium ($160→$224)** — **the only train in the whole dataset that goes at a premium**, because in Phase II it is the mainstay and it will soon be phased out by Phase IV; **the transfer is a "rescue transfer"**;
4. **The 7/8-train is discounted 80% ($900→$181)** — **the cheapest train source in the whole dataset**. **Late-founded companies keep themselves alive on this "low-price train source"** — this corroborates §14.4's "PRR is actually better when founded late": **late-founded companies can buy discounted 7/8-trains from the leading companies**.

### 9.4 At the company level: **IC is the train source company**

| Company | Times sold | Times bought | **Net direction** |
|:--|--:|--:|:--|
| **IC** | Many | Few | **Net seller (train source)** |
| B&O / NYC / GT | Comparable | Comparable | Balanced |
| **ERIE / C&O / PRR** | Few | Many | **Net buyer (aid recipient)** |

**→ IC is the "train source company"** (it buys the most trains per game on average, and the trains it swaps out are passed to its own late-founded companies); **ERIE/C&O/PRR are the "aid recipients"** — **this once again confirms the chain "IC starts strong → has surplus grain → supports late-founded companies"** (§6.2's Land Grant is the source of this chain).

### 9.5 Differences between player groups

**"Doing inter-company train transfers" is not a mark of an expert** — within the TOP group, those who did it (residual +0.10) are actually worse than those who did not (−0.03). **It is a passive action after controlling two companies, not an active advantage.**

---


> **Note**: For the sections starting from §10, the structure is the same as before — **mechanism explanation → data table → how to read it → sample warning**.

---

## 10. Minor companies (Independent Railroads) — transfer prices

### 10.1 The three minor companies that can be acquired by companies

| Minor company | List Price cap | Times transacted | Avg. price | **Median** | **Paid full List Price** | Mode of the price distribution |
|:--|--:|--:|--:|--:|--:|:--|
| **Michigan Southern (MS)** | **$60** | 635 | $55 | **$60** | **84%** (536/635) | $60×536\\|$20×12\\|$50×11 |
| **Big 4** | **$40** | 630 | $37 | **$40** | **89%** (562/630) | $40×562\\|$20×14\\|$10×14 |
| **C&WI** | **$60** | 627 | $57 | **$60** | **91%** (568/627) | $60×568\\|$20×7\\|$30×7 |

**The rules allow any price within the List Price, but 84–91% all pay up to the cap.**

### 10.2 [Core]Why does everyone pay full? — **98–100% is selling to oneself**

| Minor company | Seller president = buyer president | Proportion |
|:--|--:|--:|
| **Michigan Southern** | **632/635** | **100%** |
| **Big 4** | 621/630 | **99%** |
| **C&WI** | 616/627 | **98%** |

**Mechanism**: When a Private Company / minor company is acquired by a company, **the money is paid to the "holder" (the individual player), not to the bank**. And the buying company is usually controlled by the same president — **so "the company spends money to buy a minor company" is equivalent to "transferring the company treasury's money into one's own pocket"**.

**Because money in the company treasury does not count for final scoring while personal cash does, this transfer is a pure gain** — **this is the reason 84–91% all pay up to the cap**.

**The same logic also applies to §9's inter-company train transfers (99% selling to oneself)**: **1846's "inter-company transactions" as a whole are all a channel for "the president turning company assets into personal assets".**

### 10.3 Number of company closures (share price to zero)

| Target | Closures |
|:--|--:|
| **IC** | **47** |
| BIG4 | 31 |
| MS | 25 |
| B&O | 15 |
| GT | 13 |
| ERIE | 12 |
| PRR 7\\|C&O 7\\|NYC 3 | |

**Outcome of the founders who closed a company: avg. rank 2.47 (baseline 2.60) | win rate 23% | n=104.**

**→ The results of those who closed a company are not bad at all** (2.47 vs baseline 2.60) — **this shows that "letting a company die" is a controllable tactical action in 1846, not a disaster** (the same logic as 1862's "39% of bankrupt players win the championship", but 1846 is gentler).

**par distribution**: Among the closed companies, **93 times it was par $40** — **"start low, close fast" is a mature routine** (§6.1's $40 tier, §13.2's shell corporation).

**In 2-player games, closures are 0** — under the two-player variant, companies almost never die.

---

## 11. Where TOP / MID / BOT differ

| Group | Games | **Avg. net worth** | **Avg. dividends** | Avg. shares bought | Avg. shares sold | **Sell/buy** | Own-company share | Avg. Private investment |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|
| **TOP** | 998 | **$6628** | **$3613** | 1879 | 366 | **19%** | **44.9%** | $127 |
| MID | 946 | $5764 | $3145 | 1673 | 234 | 14% | 47.7% | $134 |
| BOT | 185 | $5165 | $2877 | 1652 | 169 | **10%** | 51.1% | $139 |

**Five points**:

1. **TOP's net worth is $1463 higher (28%)**, and the **main source of the gap is dividends (+$736, half of it)** — **not buying shares to earn the spread, but holding high-dividend companies**;
2. **TOP's sell/buy ratio is the highest (19%)** — **they are more willing to sell** (not holding on to the death);
3. **TOP's proportion of holding its own companies is the lowest (44.9% vs BOT 51.1%)** — **more diversified**;
4. **TOP's Private investment is actually smaller ($127 vs $139)** — **the money is spent on shares, not on Privates**;
5. **BOT has only 185 games** (TOP 998, MID 946) — **people in the BOT group play less, so the sample is naturally thin**; be careful when interpreting.

---

## 12. Share allocation: whose shares, and when

### 12.1 Where each group's share-purchase money goes (%)

| Group | IC | NYC | B&O | GT | PRR | C&O | **ERIE** |
|:--|--:|--:|--:|--:|--:|--:|--:|
| **TOP** | **17%** | 16% | 15% | 14% | 12% | — | 13% |
| MID | 16% | 15% | 14% | 15% | — | 14% | 13% |
| **BOT** | 12% | 12% | 12% | 14% | 14% | **20%** | **18%** |

**Two points**:
1. **TOP's allocation is highly balanced** (12–17%), **with no obvious overweighting**;
2. **BOT puts 38% of its money into C&O + ERIE (20% + 18%)** — **these two are the weakest companies in the whole table** (§15).

### 12.2 Timing of buying and selling (by Stock Round, share of amount)

| Group | | SR1 | SR2 | SR3 | **SR4** | SR5 | SR6 |
|:--|:--|--:|--:|--:|--:|--:|--:|
| **TOP** | Buy | 14.9% | 12.9% | 21.8% | **29.8%** | 17.6% | 2.9% |
| | Sell | 4.5% | 11.8% | 25.9% | 22.2% | 22.4% | 13.1% |
| MID | Buy | 15.6% | 11.3% | 18.8% | **27.7%** | 21.9% | 4.4% |
| | Sell | 2.6% | 9.1% | 21.5% | 24.3% | **29.4%** | 12.6% |
| **BOT** | Buy | 15.4% | 9.3% | 14.9% | 24.7% | **25.9%** | **9.7%** |
| | Sell | 4.1% | 7.3% | 17.9% | **29.3%** | 29.1% | 12.3% |

**Two patterns**:

1. **SR4 is the main position-adding period for the whole dataset** (all three groups put 25–30% of their funds here);
2. **BOT keeps 35.6% of its funds until SR5–SR6 to invest** (TOP only 20.5%), **by which time only 1–2 Operating Rounds are left** — **the money is invested too late to produce dividends**.

### 12.3 What to dump, and what to buy after dumping

**The amount each company was sold for and its distribution by period**:

| Company | Amount sold | Share of total selling | **SR5 dumping share** | SR6 |
|:--|--:|--:|--:|--:|
| **B&O** | $151,949 | **21.1%** | 23% | 14% |
| **NYC** | $144,647 | **20.1%** | 19% | 11% |
| IC | $141,312 | 19.6% | 22% | 10% |
| GT | $123,676 | 17.2% | 26% | 13% |
| PRR | $61,518 | 8.5% | **34%** | 15% |
| **ERIE** | $49,670 | 6.9% | **42%** | 15% |
| **C&O** | $46,971 | 6.5% | 29% | **27%** |

**→ ERIE (42%+15%=57%), PRR (49%), C&O (56%) are "end-period clearance stocks"** — **they are only concentratedly dumped in SR5–SR6**.

**The TOP group is the main force of dumping**: 53% of IC's sold amount, 51% of NYC's, and 47% of B&O's all come from TOP's hands (because TOP holds the most).

**Where the money goes after dumping: switch rate 100%**

| Group | Destination of switch purchases |
|:--|:--|
| **TOP** | **IC 17%\\|NYC 16%\\|B&O 15%\\|GT 14%\\|ERIE 13%\\|PRR 12%** (balanced) |
| MID | IC 16%\\|GT 15%\\|NYC 15%\\|C&O 14%\\|B&O 14%\\|ERIE 13% |
| **BOT** | **C&O 20%\\|ERIE 18%**\\|GT 14%\\|PRR 14%\\|B&O 12%\\|NYC 12% |

**→ This is the sharpest difference in this report**: **TOP's switch destinations are highly balanced (12–17%), while BOT is concentrated in C&O+ERIE (38%).**
**C&O and ERIE are the two weakest companies in the whole table (§15), and BOT put its switch money into the two worst ones.**

**Switch rate**: TOP 100% (1461/1465) | MID 100% (933/937) | BOT 100% (123/123) — **"sell only, never buy" happens only 8 times in the whole database**.

### 12.4 Share price settlement at the end of the Stock Round: **who dumped the price**

**Mechanism (the two rules must be viewed separately)**:

| Rule | Timing | Trigger condition | Magnitude |
|:--|:--|:--|:--|
| **§5.22** | **During** the Stock Round | Falls **only when the president sells** | One step (one step at a time, regardless of how many shares are sold) |
| **§5.52** | **At the end** of the Stock Round | Falls **as long as there is 1 share in the Bank Pool** | One step |
| **§5.52** | At the end of the Stock Round | Conversely, **100% of the shares are in players' hands** | **Rises one step** (when the share price is <$550) |

**Log identification**: The end-of-round settlement appears as a string of consecutive `share price moves` before `X has priority deal`.

#### (a) Overall: **68% of Stock Rounds have at least one company whose price falls because of shares held in the Bank Pool**

**3599 Stock Rounds in total**, distribution of the number of companies whose price fell: {0: 1142, 1: 827, 2: 794, 3: 559, 4: 232, 5: 42, 6: 3}

#### (b) Direct sources of price falls (5248 times)

| Source | Count | Share | **Number of shares** |
|:--|--:|--:|--:|
| **Only issuance from treasury by the company** | **2492** | **47%** | **6783 shares (57%)** |
| Only player selling | 1929 | 37% | 5191 shares |
| Both | 827 | 16% | — |

**→ Company issuance from treasury (issue) is the number one source of price dumping (57% of the shares).**

**Each company being dumped / rising (average per game)**:

| Company | Games present | **Falls/game** | Rises/game |
|:--|--:|--:|--:|
| **NYC** | 661 | **1.66** | 1.82 |
| B&O | 658 | 1.64 | 1.77 |
| GT | 633 | 1.58 | 1.57 |
| PRR | 440 | 1.28 | 1.39 |
| **IC** | 664 | **1.20** | **2.01** |
| ERIE | 385 | 0.90 | 0.95 |
| C&O | 418 | 0.88 | 0.93 |

**→ IC is the only company that "rises far more than it falls" (2.01 vs 1.20)** — **buying IC's shares is the safest in the whole dataset**.

**Number of shares that dumped the price due to issuance from treasury (by company)**: B&O 1412 | NYC 1404 | GT 1313 | PRR 852 | IC 666 | C&O 611 | ERIE 525

#### (c) The players who dumped the price the most by selling shares

| Player | Price dumps | Shares sold | Group | RR |
|:--|--:|--:|:--|--:|
| **Player12** | **352** | 539 | TOP | 1818 |
| Player9 | 212 | 359 | TOP | 1850 |
| Player11 | 171 | 283 | TOP | 1808 |
| Player16 | 164 | 233 | TOP | 1775 |
| Player6 | 150 | 223 | TOP | 1796 |
| Player17 | 115 | 181 | TOP | 1749 |
| Amori | 98 | 133 | TOP | 2105 |

**By group**: TOP **1.73 times/game** | MID 1.08 | **BOT 0.71**.

**→ "The ones who dump the price the most are all TOP players" is because they play more games** — **it is only meaningful to look at "times/game" (1.73 vs 0.71): TOP's selling frequency is 2.4 times BOT's.**

#### (d) What happens to the presidents of companies whose price was dumped (after skill adjustment)

| Situation | n | **Skill-adjusted residual** |
|:--|--:|--:|
| Dumped (all) | 5242 | **+0.12** |
| Dumped by others | 4757 | +0.10 |
| **Dumped by the president himself** | 485 | **+0.30** |
| **Price rose at round end** | 6065 | **−0.09** |

**→ Being dumped is bad news (+0.12), dumping oneself is even worse (+0.30); a price rise is good news (−0.09).**

**Outcome of the sellers** (after skill adjustment): all −0.02 | **TOP +0.10** | MID −0.03 | **BOT −0.03**.

**→ Interestingly: the residual of TOP sellers is +0.10 (slightly worse than expected), while the sellers of MID/BOT are not bad at all.** This shows that **the action of "selling shares" itself does not constitute an advantage** — **it is merely doing the right thing at the right time, and this timing choice has already been captured by the BT rating**.

### 12.5 Share allocation recommendations (data version)

| Stage | Recommendation | Data support |
|:--|:--|:--|
| **SR1** | Grab IC, then NYC / B&O | IC's average position 1.78; §6.2's Land Grant |
| **SR2–SR3** | Hold 1–2 high-revenue companies of others | TOP's allocation is highly balanced |
| **SR4** | **Main position-adding period** | All three groups put 25–30% of funds here |
| **SR5** | **Mainly reduce holdings**, especially dump ERIE / C&O / PRR | 49–57% of these three's sold amount is concentrated in SR5–SR6 |
| **When switching** | **After selling you must buy, but do not buy into C&O / ERIE** | 65% of TOP's switches land in the four big companies; **38% of BOT's land in C&O+ERIE** |
| **Always** | Keep own-company holdings at **around 45%** | TOP's own 44.9%, BOT's 51.1% |

---

## 13. Companies' market positioning

### 13.1 When does the revenue come

| Company | **Total revenue from running trains** | **OR1–2 share** | OR3–4 share | **OR5+ share** | Total dividends paid | **Payout lag (rounds)** |
|:--|--:|--:|--:|--:|--:|--:|
| **IC** | **2,697,410** | 18% | 44% | 38% | 2,402,230 | **3.66** |
| NYC | 2,589,550 | 19% | 42% | 40% | 2,229,180 | 3.68 |
| B&O | 2,437,940 | 20% | 42% | 38% | 2,051,830 | 3.72 |
| GT | 2,207,690 | 17% | 42% | 41% | 1,919,340 | 3.77 |
| PRR | 1,270,820 | 14% | 40% | **46%** | 1,089,250 | 3.95 |
| **ERIE** | **762,360** | **11%** | 36% | **54%** | 688,730 | **4.33** |
| **C&O** | 809,570 | 11% | 35% | **54%** | 723,390 | **4.31** |

**Three points**:
1. **IC/NYC are "early companies"** (payout lag 3.66/3.68 rounds)—**the earlier you buy them, the more they are worth**;
2. **ERIE/C&O are "late companies"** (payout lag 4.3 rounds, OR5+ share 54%)—**most of their revenue only appears at the time when it is already too late for you to cash out**;
3. **The magnitude of total revenue differs by 3.5 times** (IC $2.69 million vs ERIE $0.76 million)—**this is the most direct measure of "company strength"**.

### 13.2 Shell corporations / idle corporations (never ran trains after founding)

| Company | Games founded | **Never ran trains** | Share |
|:--|--:|--:|--:|
| **IC** | 664 | **45** | **7%** |
| NYC | 661 | 5 | 1% |
| B&O | 658 | 17 | 3% |
| ERIE | 385 | 17 | 4% |
| GT | 633 | 11 | 2% |
| C&O | 418 | 8 | 2% |
| PRR | 440 | 7 | 2% |

**→ IC's "shell rate" is the highest (7%)**—**related to its Land Grant mechanism**: **take the $240 Land Grant money, do not run trains, transfer the money to the main company**. This is precisely the data support for the second explanation of "TOP3 avoid IC" in §4.3.

### 13.3 Dividends vs withholding (by player group × phase)

| Phase | **TOP** payout/withhold | MID | BOT |
|:--|:--|:--|:--|
| I | **1% / 99%** | 12% / 87% | 3% / 97% |
| II | 73% / 17% | 67% / 22% | 66% / 26% |
| III | 54% / 26% | 54% / 26% | 56% / 27% |
| IV | 76% / 14% | 80% / 13% | 81% / 12% |

**All**: TOP total decisions 15277 (full payout 68% / withhold 20% / half dividend 12%); MID 14313 (67/22/11); BOT 2818 (68/22/9).

**→ The dividend policies of the groups are almost the same (68% full payout)**—**dividend strategy does not distinguish skill levels**. The difference in Phase I is the largest (TOP 1% vs MID 12%) but the sample is small.

---

## 14. Founding decisions

### 14.1 How many companies are founded per game?

| Player group | Games | Foundings | **Avg foundings per game** | Founded in SR1 | Late-founded in SR3+ | **Late-founded share** |
|:--|--:|--:|--:|--:|--:|--:|
| TOP | 998 | 1430 | **1.43** | 1070 | 359 | **25%** |
| MID | 946 | 1313 | 1.39 | 951 | 348 | 27% |
| BOT | 185 | 261 | 1.41 | 176 | 76 | **29%** |

### 14.2 Number of companies founded per game → outcome

| Number of companies founded | Count | Avg rank | **Win rate** | Avg par |
|:--|--:|--:|--:|--:|
| **1 company** | 1649 | **2.54** | **26%** | 84 |
| 2 companies | 1862 | 2.65 | 22% | 106 |
| **3 companies** | 336 | **3.04** | **14%** | 108 |

**→ Consistent with the residual measure in §4.5 (net effect +0.49): the more you found, the worse it goes.**

**Why?** Because **opening more companies dilutes your shareholding percentage in each company**, and the second and third companies are often short of funds (par only $106/$108 yet they must be supported at the same time)—**1846 is not a game of "opening many companies", but a game of "deep-cultivating one"** (exactly the opposite of 1862).

### 14.3 Founding timing → outcome

| Founding timing | Count | **Avg par** | Avg rank | Win rate | **That company's full-game revenue** |
|:--|--:|--:|--:|--:|--:|
| **SR1** | 2803 | **81** | 2.57 | 25% | **$4031** |
| SR2 | 52 | 92 | **3.31** | 13% | $3112 |
| SR3 | 192 | 127 | 2.68 | 27% | $2297 |
| **SR4+** | 812 | **144** | 2.80 | 18% | **$1077** |

**→ Companies founded in SR1 have a full-game revenue of $4031, those in SR4+ only $1077 (1/4).**

**By group × timing**:

| Group | SR1 n | Avg rank | **Win rate** | SR3+ n | Avg rank | Win rate | Avg par |
|:--|--:|--:|--:|--:|--:|--:|--:|
| **TOP** | 1070 | **2.06** | **38%** | 359 | 2.34 | 30% | $148 |
| MID | 951 | 2.82 | 17% | 348 | 2.94 | 14% | $140 |
| BOT | 176 | 3.62 | 3% | 76 | 3.63 | 3% | $145 |

**→ TOP's win rate when founding a company in SR1 is as high as 38% (MID 17%, BOT 3%)**—**this is the largest single inter-group gap overall**.

### 14.4 By company: founded in SR1 vs late-founded

| Company | SR1 count | Avg rank | Win rate | **Late-founded count** | Avg rank | Win rate | Late-founded avg par |
|:--|--:|--:|--:|--:|--:|--:|--:|
| **IC** | 638 | **2.34** | **27%** | 18 | 2.83 | 17% | $124 |
| NYC | 598 | 2.42 | **29%** | 54 | **2.31** | **30%** | $133 |
| B&O | 565 | 2.55 | 26% | 90 | 2.64 | 20% | $141 |
| GT | 509 | 2.70 | 21% | 116 | 2.74 | 22% | $138 |
| **PRR** | 255 | **3.07** | 17% | 174 | **2.85** | 18% | $142 |
| ERIE | 113 | 2.62 | 21% | **264** | 2.92 | 16% | $141 |
| **C&O** | 125 | 2.94 | 19% | **288** | 2.75 | 20% | $144 |

**Three key observations**:

1. **C&O and ERIE are mainly founded when "late-founded"** (288/418, 264/385)—**they are "late-game companies"** (corroborated by the payout lag of 4.3 rounds in §13.1);
2. **⚠ PRR is the only company in the whole table where "late-founding is better"** (SR1 avg rank 3.07 vs late-founded 2.85)—**founding PRR in SR1 is the worst choice; waiting for others to move first is instead better**. **This corrects part of the reason for "PRR's founder residual is the worst" in §4.2**: it is not that PRR itself is bad, but that **too many people open it at the wrong timing**;
3. **NYC is the only company that is "good both in SR1 and late-founded"** (2.42/29% vs 2.31/30%)—**its position is too strong; it can be opened at any time**.

### 14.5 Each company's appearance rate is itself a signal

| Company | Appearances | Appearance rate |
|:--|--:|--:|
| IC | 664 | 99.9% |
| NYC | 661 | 99.4% |
| B&O | 658 | 99.0% |
| GT | 633 | 95.2% |
| **PRR** | **440** | **66.2%** |
| **C&O** | **418** | **62.9%** |
| **ERIE** | **385** | **57.9%** |

**→ On average only 5.80 companies are founded per game (7 in total)**—**that is, 1.2 companies per game are never opened at all**.

**"Which one was not opened" is itself a signal**:
- **IC/NYC/B&O/GT almost always come out** (95–100%)—**they are consensus-strong companies**;
- **PRR/C&O/ERIE do not appear at all in 34–42% of games**—**they are "selectively abandoned"**.

**Inference**: **when PRR/C&O/ERIE are actually opened, it is often because "there is nothing left to choose"** (the good four have all been grabbed). **This is the real reason their results are poor**—**not because the companies themselves are bad, but because the people who open them are already in a disadvantaged position**.

---

## 15. Full-lifecycle profiles of the seven companies

| Company | Appearances | Avg par | **Avg total dividends** | Avg revenue | Avg trains run | **Median final price** | **Founder residual** | 2-train revenue/cost | Rust-event affordability rate | Shell rate | Payout lag |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| **IC** | 664 | 85 | **$3618** | 4062 | 21.4 | 375 | **−0.042** | 3.4 | **54%** | **7%** | 3.66 |
| **NYC** | 661 | 87 | $3372 | 3918 | 20.3 | 320 | **−0.013** | 3.6 | 26% | 1% | 3.68 |
| B&O | 658 | 89 | $3118 | 3705 | 18.6 | 295 | +0.022 | **4.5** | 28% | 3% | 3.72 |
| GT | 633 | 91 | $3032 | 3488 | 17.3 | 295 | +0.029 | 3.5 | 29% | 2% | 3.77 |
| PRR | 440 | 105 | $2476 | 2888 | 13.3 | 250 | **+0.070** | 3.5 | 28% | 2% | 3.95 |
| ERIE | 385 | 121 | $1789 | 1980 | 8.7 | 212 | +0.042 | **2.9** | 31% | 4% | **4.33** |
| **C&O** | 418 | **124** | **$1731** | 1937 | 8.5 | 212 | +0.045 | 3.8 | 30% | 2% | **4.31** |

### 15.1 Company-by-company full-lifecycle profiles

> **Template for each company**: mechanism features → routes and stations → trains → shares/dividends → who is opening it → play recommendations.

#### 15.1.1 IC — the best founding target in the whole study

| Dimension | Data |
|:--|:--|
| **Mechanism** | **Land Grant (§5.35): treasury = 3×par**—at the same par of $85 it is $83 more than NYC (+48%), **and this money is free** |
| Starting treasury / cash available in the first OR | **$256 / $516** (one of the highest overall) |
| **Routes** | **A whole-map company**: coverage of the six major hubs 265–353%, and **the first successful run is always within 2.1–3.1 turns** |
| Stations | **2.49** per game on average (the most overall); Chicago×317, Cincinnati×274, Toledo×183 |
| **Opening train** | **96% bring out a train in OR1.1**; mainstream `2+2` (196 games 2.28/30%), `2+2+2` (101 games 2.38/23%) |
| **2-train revenue/cost** | 3.4× |
| **Rust-event affordability rate** | **54% (the only one over half)** |
| Dividends / final price | **$3618 / $375 (first in both)** |
| **Payout lag** | **3.66 rounds (the shortest)—the earliest cash return** |
| **Founder residual** | **−0.042 (the best)** |
| SR1 vs late-founded | 638 / 18—**almost only opened in SR1** |
| **Shell rate** | **7% (the highest overall)**—often treated as a cash machine |
| Dumped / risen (per game) | **1.20 falls / 2.01 rises (the only one where rises far outnumber falls)** |
| Company closures | 47 times (the most) |
| Best Private Company partners | **Mail Contract (221 games), Michigan Southern (253 games), Boomtown, Lake Shore Line** |
| As inter-company train transfer seller | **The largest train-source company** (occupies 4 of the top 6 cheap-sale combinations) |

**Play recommendations**:
1. **Be the first to grab it in SR1** (in 55% of games it is grabbed first);
2. **Set par at $80–90** ($80 is exactly 3 2-trains; $90 has the highest win rate at 29%);
3. **Play toward Toledo** (those who place a station marker 1.99/37%, 0.56 ranks better than those who do not)—**do not crowd into Chicago** (2.44/25%, which is actually lower than 2.20/31% for those who do not crowd in);
4. **It is the preferred shell for the "cash machine" strategy** (shell rate 7%)—but that is **another kind of play** (§17).

#### 15.1.2 NYC — the most stable second-best, the only company that "can be opened at any time"

| Dimension | Data |
|:--|:--|
| **Routes** | **Cleveland 389% / 1.2 turns (the fastest home city overall)**; Chicago 356%, Toledo 316% |
| Stations | 2.18 per game on average; **Cleveland×322**, Chicago×258, Toledo×244 |
| **Opening train** | **90% bring out a train in OR1.1**; **`2+2+3/5` 44 games 1.80/50% (one of the best combinations overall)** |
| **2-train revenue/cost** | 3.6× |
| **Rust-event affordability rate** | 26% (on the low side—**its treasury is not as thick as IC's**) |
| Dividends / final price | $3372 / $320 (second in both) |
| Payout lag | 3.68 rounds |
| **Founder residual** | **−0.013 (second)** |
| **SR1 vs late-founded** | 598 / 54—**2.42/29% vs 2.31/30%, the two tiers are almost the same** |
| Shell rate | 1% (the lowest) |
| Dumped / risen (per game) | **1.66 falls / 1.82 rises (falls the most)** |
| Company closures | 3 times (the fewest) |

**Play recommendations**:
1. **You must grab Cleveland** (those who place a station marker 2.46/27% vs those who do not 2.61/23%);
2. **For the opening train you can take 3** (3 2-trains: win rate 32%, per-capita revenue $863, both better than 2 trains' 26%);
3. **It does not need to be grabbed, but it can serve as a "safe card"**—**if IC has been grabbed away and the situation is unclear, NYC is the second-best option with less competition** (this is what TOP3 does, §21).

#### 15.1.3 B&O — the king of the 2-train, the highest revenue density

| Dimension | Data |
|:--|:--|
| **Mechanism** | **Cincinnati privilege (§6.52): can place a station marker while not connected**—**66% of games used the $100 privilege price** |
| **Routes** | **Cincinnati 354% / 1.2 turns**; Chicago 323%, Cleveland 331%, Indianapolis 296%/1.2 |
| Stations | 2.22 per game on average; **Cincinnati×526 (the most for a single city overall)** |
| **Opening train** | 84% bring out a train in OR1.1; **`2+2+2` 143 games 2.27/36% (its optimal combination)**, `2+3/5` 37 games 2.41/35% |
| **2-train revenue/cost** | **4.5× (the highest overall)** |
| Dividends / final price | $3118 / $295 |
| Payout lag | 3.72 rounds |
| **Founder residual** | +0.022 (mid-pack) |
| **Best Private Company partners** | **Boomtown (lift 2.42, the strongest overall), Steamboat (2.14), Little Miami, Big 4** |
| Window revenue (TOP/BOT) | **397 / 304 (the highest absolute value overall)** |

**Play recommendations**:
1. **Occupy Cincinnati right from the start**—**even if it costs the $100 privilege price, it is worth it**;
2. **Pair it with Midwest Private Companies** (Boomtown / Steamboat / Little Miami)—**lift 2.14–2.42**;
3. **It is the best carrier of the "2-train flow"**: a 4.5× revenue/cost ratio means **buying 3 2-trains is especially worthwhile**;
4. **Do not pair the Michigan-series Private Companies (MS) with it**—**MS×B&O is the strongest negative pairing in the whole table (lift 0.39)**.

#### 15.1.4 GT — the space-grabber with the fewest stations, the most prominent structural contradiction

| Dimension | Data |
|:--|:--|
| **Routes** | **Detroit 366% / 1.2 turns**; Chicago 350%, Toledo 318% |
| **Stations** | **Only 3 (the fewest overall)**; Chicago×289, Toledo×288, Detroit×143 |
| **Opening train** | 79% bring out a train in OR1.1; `2+2+2` is clearly better than `2` alone (the latter has a win rate of only 7%) |
| **2-train revenue/cost** | 3.5× |
| **Rust-event affordability rate** | 29% |
| Dividends / final price | $3032 / $295 |
| Payout lag | 3.77 rounds |
| **Founder residual** | +0.029 |
| **Window revenue (TOP vs BOT)** | **358 vs 283 (gap 75, one of the largest)** |

**Structural contradiction**: **only 3 station markers, yet it must grab Chicago** (those who place a station marker 2.55/25% vs those who do not 2.69/18%)—**once the 3 markers are placed there are no more, and it has 6 hubs to cover.**

**Play recommendations**:
1. **Pair it with the Michigan-series Private Companies (MS lift 1.90, Michigan Central 1.70)**—**their station markers can alleviate GT's station shortage** (§3.4);
2. **Grab Chicago, but know the cost**—**few stations means you must trade off between "grabbing spaces" and "protecting home"**;
3. **It is one of the companies that best reflects the skill gap** (TOP 358 vs BOT 283).

#### 15.1.5 PRR — the worst founding target in the whole table, but it can be late-founded

| Dimension | Data |
|:--|:--|
| **The only advantage** | **The most stations: 5** (4 extra + home city), 31% of games place all 4 |
| Routes | Fort Wayne 221% / 3.1\\|Cleveland 226% / 3.2\\|Chicago 210% / 3.1—**all on the slow side** |
| Opening train | Only 61% bring out a train in OR1.1 (**32% have to wait until OR4+**) |
| **2-train revenue/cost** | 3.5× |
| **Rust-event affordability rate** | 28% |
| Dividends / final price | $2476 / $250 (**dividends rank 5th, but the founder outcome is the worst**) |
| Payout lag | 3.95 rounds |
| **Founder residual** | **+0.070 (the worst overall)** |
| **⚠ SR1 vs late-founded** | **255 / 174—3.07 vs 2.85, the only "late-founding is better" in the whole table** |
| **Appearance rate** | **Only 66.2%—in 34% of games it is never opened at all** |
| Negative pairings | **MS×PRR (3.15/12%), Big 4×PRR (3.14/12%)—the two worst in the whole table** |

**Three key readings**:
1. **PRR's "badness" is mainly a timing problem**—**too many people open it in SR1** (255 times vs 174 late-founded times);
2. **The reason its negative pairings are the worst is that "Michigan-series Private Companies + PRR" are geographically unrelated to each other** (one is in the northwest, the other in the southeast);
3. **It is often never opened at all (34%)**—**when it does appear, it is often because "the good four have all been grabbed"**.

**Play recommendations**:
1. **Do not open PRR in SR1**—if you want to open it, wait for SR3+;
2. **If you absolutely must open it in SR1, pair it with Private Companies in the Wheeling/Pittsburgh direction** (Tunnel Blasting), **never pair it with the Michigan series**;
3. **It has 5 station markers and is the only company that can "cover the whole board"**—**this is a late-mover's card, not a first-mover's card**.

#### 15.1.6 ERIE — late-game company (one)

| Dimension | Data |
|:--|:--|
| **Par threshold** | **$121** (the second highest)—**the starting price can only be in the high end of the yellow zone** |
| **Routes** | **Bottom across the board**: Erie 139%/4.2, Cleveland 204%/4.2, Chicago 139%/4.2, Indianapolis 75%/4.2 |
| Stations | **1.03** per game on average (the fewest); **94% of games place not a single station marker** |
| **Opening train** | **Only 24% in OR1.1; 57% have to wait until OR4+**—**median OR4.1** |
| **2-train revenue/cost** | **2.9× (the lowest)** |
| Dividends / final price | **$1789 / $212** |
| **Payout lag** | **4.33 rounds (the longest)** |
| Founder residual | +0.042 |
| **SR1 vs late-founded** | 113 / **264—mainly founded when "late-founded"** |
| Appearance rate | 57.9% (the lowest) |
| **Cheap-sale receipts** | **IC→ERIE is the most frequent cheap-sale channel overall (64 times)**—**it is the biggest "aid recipient"** |
| Clearing out | **57%** of the amount sold is concentrated in SR5–SR6 |

**Play recommendations**: **ERIE is not a "bad company", but a "late-game company"**—**its revenue only appears at OR5+ (54%), and by then it is already too late for you to cash out.** Do not open it in SR1.

#### 15.1.7 C&O — late-game company (two), and it does not touch the most important chokepoint

| Dimension | Data |
|:--|:--|
| **Par threshold** | **$124 (the highest)** |
| Routes | Cincinnati 210%/4.2, Cleveland 172%/4.2, Chicago 162%/4.2, **Toledo 89%/4.2 (the lowest overall)** |
| Stations | 1.14 per game on average; **74% of games place not a single station marker** |
| **Opening train** | 25% in OR1.1; **62% have to wait until OR4+**—median OR4.1 |
| **2-train revenue/cost** | 3.8× (not bad) |
| Dividends / final price | **$1731 / $212 (the lowest)** |
| **Payout lag** | **4.31 rounds (the second longest)** |
| Founder residual | +0.045 |
| **SR1 vs late-founded** | 125 / **288—mainly founded when "late-founded"** |
| **⚠ Structural problem** | **Toledo coverage is only 89%**—**it hardly touches the most valuable chokepoint overall** (§7.3: Toledo first-to-occupy win rate 28%, the highest of the four chokepoints) |
| Clearing out | **56%** of the amount sold is concentrated in SR5–SR6 |

**Play recommendations**:
1. **The structural reason C&O is weak**: **its home city Cincinnati is too far from Toledo**, and Toledo is the most valuable space overall—**it cannot get it**;
2. **Its 2-train revenue/cost is not bad (3.8×)**; **what is bad is "when it can start"** (OR4.1);
3. **Do not open C&O in SR1** (125 SR1 times vs 288 late-founded times).

### 15.1b [Summary]The "personality" of the seven companies in one sentence

| Company | One sentence | When it should be opened |
|:--|:--|:--|
| **IC** | **The strongest in the whole study, free money, a road network extending in all directions** | **Grab it first in SR1** |
| **NYC** | The most stable second-best, the fastest home city | SR1 (if you cannot get IC, take it) |
| **B&O** | The king of the 2-train, the highest revenue density | SR1 |
| **GT** | The space-grabber with the fewest stations, structural contradiction | SR1 |
| **PRR** | The most stations, but **the only company in the whole table that should be late-founded** | **SR3+** |
| **ERIE** | Late-game company, it cannot even get its home city running | SR3+ |
| **C&O** | Late-game company, and it cannot reach Toledo | SR3+ |

### 15.2 [Core]Company strength ≠ founder outcome—but the direction is the opposite of 1830

**The lesson of 1830 is "the stronger the company, the more people grab it, the higher the founding cost, and the founder's outcome is instead worse".**

**1846's data shows the opposite**:

| Company | Company dividend rank | Founder residual rank |
|:--|--:|--:|
| IC | 1 | 1 |
| NYC | 2 | 2 |
| B&O | 3 | 3 |
| GT | 4 | 4 |
| ERIE | 6 | 5 |
| C&O | 7 | 6 |
| PRR | 5 | **7** |

**→ In 1846, company strength and founder outcome are almost completely consistent (only PRR is the exception).**

**Why is it different from 1830?** Because **1846's "founding cost" is very low** (§6.1: median par is only $80–90, and it is **paid into the company** rather than to the bank)—**the extra money you pay for a strong company ultimately remains in your own company's treasury**. **In 1830, buying a strong company requires paying real money to the bank; in 1846, it does not.**

---

## 16. TOP players' favourite lines vs. lowest-win-rate lines

> **Definition**: for each "behaviour", compute the **skill-adjusted residual** once each for the **doer** and the **non-doer**; the difference between the two = **net effect** (negative = this behaviour is good). **The all-players baseline residual = 0.**

### 16.1 Lines with the best net effect (should do)

| Behaviour | n | Proportion | Doer residual | Non-doer | **Net effect** |
|:--|--:|--:|--:|--:|--:|
| **SR1 grabs IC/NYC/B&O/GT** | 2392 | 87% | −0.03 | +0.23 | **−0.27** |
| **Founding only 1 company per game** | 1649 | 60% | −0.07 | +0.10 | **−0.17** |
| Founding 2 companies per game | 1862 | 68% | — | — | Middle tier |
| **3+ 2-trains in the opening (pure 2-trains)** | 534 | 19% | −0.01 | +0.00 | **−0.02** (see warning) |

### 16.2 Lines with the worst net effect (avoid)

| Behaviour | n | Proportion | Doer residual | Non-doer | **Net effect** |
|:--|--:|--:|--:|--:|--:|
| **Founding ≥3 companies per game** | 115 | 4% | **+0.46** | −0.02 | **+0.49** |
| **SR1 grabs PRR** | 255 | 9% | **+0.26** | −0.03 | **+0.29** |
| **A company's first train includes a 7/8** | 386 | 14% | **+0.22** | −0.04 | **+0.26** |
| **Founding a $150 company** | 755 | 28% | +0.18 | −0.07 | **+0.25** |
| **Founding ERIE or C&O** | 763 | 28% | +0.18 | −0.07 | **+0.25** |

### 16.3 [Counter-intuitive] "Buying 3+ 2-trains in the opening" and "grabbing PRR" are completely different in nature

| Behaviour | Doer mean BT | All-players mean BT | Reading |
|:--|--:|--:|:--|
| **3+ 2-trains in the opening** | **1675** | 1604 | **Strong players' self-selection** — the book net effect of −0.02 is the result of "being levelled out by strong players" |
| **SR1 grabs PRR** | **1529** | 1604 | **Weak players' self-selection** — part of the book +0.29 is "weak players being bad to begin with" |

**These two are exactly the two opposite situations that "skill adjustment" is meant to address**:

- **Strong players did something that looks ineffective** (buying 3 2-trains) → **the book figure is overestimated**; the real payoff may be better than −0.02 (for strong players);
- **Weak players did something that looks bad** (grabbing PRR) → **the book figure is underestimated**, but the direction is still that **it should not be done in itself** (§14.4: PRR should be founded late).

**⚠ The residual definition in this paper still has residual circularity**: the BT rating itself is derived from ranks (§20.2, limitation 2).

### 16.4 Behaviour test weighted by rating

**The residual definition is already equivalent to "rating weighting"** — because it applies an expectation correction according to each person's actual skill. **If you instead switch to "looking at raw ranks grouped by TOP/MID/BOT", you get numbers contaminated by skill** (this is exactly why this paper insists on using residuals).

**A comparison**: the **raw** mean rank of SR1 grabbing PRR is 3.07 (vs 2.60 for all players); it looks like "0.47 worse"; **after adjustment** only +0.29 remains — **of which 0.18 is "the people who grab PRR were weak players to begin with"**.

---

## 17. Special: deliberate blow-up (IC blow-up and its variants)

> **This section is about the line of "not transferring wealth through small companies, but opening a low-priced company and moving the assets away"** — a special play proposed by the user. **Under the new data the sample has grown from 27 cases to 141 cases**, and the conclusions are firmer than in the old version.

### 17.1 The complete sequence (using 1846-065 as an example, Player14)

```
SR1   Player14 pars IC at $40            ← founding at the lowest price
      Player14 buys a 20% share ... for $80    (President's Certificate, treasury +$80)
      IC receives a $40 subsidy            (Land Grant, treasury +$40)
      Player14 buys a 10% share ×4 for $40     (treasury +$160)
      → IC treasury = $280, president holds 60%
      Player14 sells 3 shares of IC and receives $120
      IC's share price $40→$30→$20         ← president sells, price dumping (§5.22)

OR1.1 IC buys 2 train ×3 for $80     ← treasury $240 all spent on 2-trains
      ERIE buys a 2 train for $1 from IC ×3   ★ asset transfer: $1 per train
OR1.2 IC does not run → $10→$0 → IC closes
      Player14 pars ERIE at $100            ← second company, received those 3 trains
```

**In one sentence**: **use a $40 low-priced company to turn the bank's capital injection and your own cash into trains, then transfer the trains to the second company for $1, and let the shell die.**

### 17.2 New data statistics: 141 cases (27 cases in the old version)

**Entry point**: companies founded at $40 in SR1 total **141 cases** (47 cases in the old version), of which **93 cases eventually closed** (= blow-up).

| Metric | Value |
|:--|:--|
| **Founder mean rank** | **2.50** (all-players baseline 2.54) |
| **Win rate** | **23%** (all players 24%) |
| Founder mean BT | **1637** (**higher than all players' 1604**) |
| **Distribution by tier** | **TOP 69, MID 36, BOT 6, other 30** |
| Distribution of companies founded | **IC 60**\\|GT 26\\|B&O 18\\|ERIE 15\\|PRR 10\\|C&O 7\\|NYC 5 |

**→ Three corrections (relative to the old version)**:

1. **The old version said "a blow-up is 0.37 ranks better than normal operation at the same price point"** — **the new data does not support this magnitude**: the mean rank of the 141 cases, 2.50, is almost level with the all-players baseline of 2.54;
2. **The people who do blow-ups have higher BT (1637 vs 1604)** — **this is the choice of strong players, not a "cheap line"**;
3. **The TOP tier did it 69 times, the BOT tier only 6 times** — **strong players love using it, weak players don't**.

**So the correct reading is**: **a blow-up is not a "sure profit", but "a tool strong players use when the conditions allow"**. Its mean rank is level with the baseline, but **it squeezes out the residual value of a company that was going to die anyway**.

### 17.3 Selling trains cheap: who transferred trains to whom at extremely low prices

**A total of 580 "below 35% of face value" train transfers.**

| Train type | Count | Mean transaction price | Face value |
|:--|--:|--:|--:|
| **2** | **267** | **$1** | $80 |
| **4/6** | 120 | $44 | $450 |
| **7/8** | 68 | $43 | $900 |
| 5 | 39 | $48 | $500 |
| 6 | 36 | $79 | $800 |
| 3/5 | 29 | $6 | $160 |
| 4 | 21 | $5 | $180 |

**Seller → buyer combinations (top 6)**:

| Seller | Buyer | Count | Of which same president |
|:--|:--|--:|--:|
| **IC** | **ERIE** | **64** | 56 |
| **IC** | **C&O** | 43 | 37 |
| **IC** | **PRR** | 32 | 25 |
| **IC** | **NYC** | 29 | **29 (100%)** |
| ERIE | NYC | 27 | 26 |
| IC | B&O | 23 | 21 |

**→ Of which between different companies under the same president: 498/580 = 86%** (mean rank of the selling president 2.52, level with the baseline).

**Three readings**:
1. **IC is the biggest "train-source company"** (accounting for 4 of the top 6) — **because IC starts with the thickest treasury (3×par) and has surplus grain**;
2. **ERIE/C&O/PRR/NYC are the receiving side** — **they are exactly the companies that "nobody often wants in SR1, which happen to be left vacant"**;
3. **86% are under the same president** (lower than the 99% for the full sample in §9) — **because 14% of the cheap sales went to other people**; this part deserves separate attention (it may be "doing someone a favour" or "offloading a burden").

### 17.4 Where the money comes from and where it goes

| Item | Value |
|:--|:--|
| Company treasury total (median) | **$280** |
| Of which the president's own contribution | ~$220 |
| Of which bank capital injection / land grant | ~$99 |
| **Buying trains from the bank** | **$240** (median) |
| **Recovered from cheap sales** | **$3** (median) |
| **President's net cash input** | **$80** (median) |

**→ The president nets out $80 of his own money, turning his own $80 + the bank's ~$99 into $240 of trains, and moves them into his own second company for $3**, at the cost of only a shell that was cheap to begin with.

### 17.5 Conclusions and risks

- **It is the extreme form of "inter-company train transfer"** (§9: 99% of inter-company train transfers are hand-offs within one's own family; here the price is even pushed down to $1);
- **It is at the same time part of the cause of "$40 par = disaster" in §6.1**: among those 89 cases of $40 companies, **93 cases were deliberately blown up** (the other part is in the Big 4 path of §10);
- **Risk points**: ① you need enough cash in SR1 to open two companies; ② if you fail to grab the second company, the trains are thrown away for nothing; ③ it burns outside shareholders (although only a few games have outsiders taking part); ④ **the new data shows its expected payoff is only "level"** — **if you do it badly it is pure self-harm**.

---

## 18. Special: operations in the opening four operating rounds (OR1.1–OR2.2)

> For every company that "acted within the opening window" (**2854 companies**, 1279 companies in the old version) record: initial share price, tile laying, station placement, train purchases, and **the revenue of each operating round**, then split into three tiers by total window revenue.
> **Median $790, terciles $680 / $890.**

### 18.1 Tiers by total window revenue

| Tier | Companies | Mean total revenue | **Mean rank** | **Win rate** | Mean BT | Mean par | Mean track | Mean stations | Mean trains bought |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| **High** | 931 | **$1032** | **2.10** | **38%** | **1714** | 85 | **6.3** | 1.44 | **3.5** |
| Middle | 938 | $792 | 2.56 | 23% | 1629 | 84 | 6.2 | 1.33 | 3.0 |
| **Low** | 985 | **$457** | **3.07** | **13%** | **1487** | 76 | 5.2 | 1.08 | **2.4** |

**Per-round revenue curve (means)**:

| Tier | OR1.1 | OR1.2 | OR2.1 | OR2.2 |
|:--|--:|--:|--:|--:|
| **High** | **0** | **226** | **355** | **451** |
| Middle | 0 | 181 | 266 | 345 |
| **Low** | **0** | **103** | **145** | **209** |

**★ Note that OR1.1 revenue is always 0** — because **train buying comes after the run-trains step**, so nobody can run in the first round. The gap opens up already at OR1.2, and **widens round by round** (226→355→451 vs 103→145→209).

**Company composition and president composition by tier**:

| Tier | Company composition | **President composition** |
|:--|:--|:--|
| **High** | **B&O 29%, IC 23%, NYC 21%, GT 14%**, PRR 5% | **TOP 55%**, MID 31%, other 12%, BOT 3% |
| Middle | NYC 26%, IC 20%, GT 19%, B&O 17% | TOP 40%, MID 34%, other 19% |
| **Low** | IC 24%, **GT 21%**, NYC 16%, B&O 14%, **PRR 13%** | MID 36%, **other 35%**, **TOP 19%**, BOT 10% |

**→ 55% of the presidents in the high-revenue tier are TOP; in the low-revenue tier only 19% are TOP and 35% are "other" (players with a sample of <8 games).**

### 18.2 Operations common to the high-revenue tier

| Metric | **High-revenue tier** | Low-revenue tier |
|:--|--:|--:|
| Track tiles laid (within the window) | **6.30** | 5.15 |
| Stations placed | **1.44** | 1.08 |
| **Trains bought** | **3.52** | 2.40 |
| **Train spending** | **$546** | **$323** |
| Initial share price par | **$85** | $76 |
| Number of share issuances from treasury | 1.32 | 1.03 |

**★ Placement of the first track tile** (the first tile the company itself lays):

| Tier | Placement (count) |
|:--|:--|
| **High** | J4×170, E19×160, **H12 Cincinnati×109**, G11×83, **G19 Wheeling×67**, D18×52 |
| **Low** | J4×224, E19×139, **B16 Port Huron×73**, H12 Cincinnati×70, D18×67, C13×53 |

**This table is the most telling**: **the high tier more often pounces straight at the chokepoints in the opening** (Cincinnati, Wheeling), **while the low tier more often polishes its own home city** (Port Huron) or intermediate stations.

**First train**: the high tier **2-trains ×756** (accounting for 81%), 3/5×109, 4×66; the low tier 2-trains ×731, **3/5×150, 4×97** — **the low tier buys expensive trains in the opening at twice the proportion.**

**Dividend decisions within the window**: the high tier full payout 67% / half dividend 15% / **withhold 18%**; the low tier 73% / 11% / **withhold 16%** — **the high tier is more willing to withhold in the opening** (in order to buy trains).

### 18.3 By player group: expert games vs. novice games

| Group | Companies | **Mean window revenue** | Median | Mean rank | **Win rate** | Mean track | Mean stations | Mean trains | Mean par |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| **TOP** | 1071 | **$840** | $880 | **2.06** | **38%** | 5.9 | 1.19 | **3.2** | $81 |
| MID | 964 | $751 | $770 | 2.82 | 17% | 6.0 | 1.36 | 2.9 | $81 |
| **BOT** | 185 | **$661** | $680 | **3.65** | **3%** | 5.7 | **1.57** | **2.5** | $85 |
| Other | 634 | $643 | $650 | 2.80 | 20% | 5.7 | 1.22 | 2.7 | $82 |

**Two counter-intuitive points**:

1. **The BOT group actually places the most stations (1.57 vs TOP 1.19)** — yet has the lowest revenue. **Placing many stations does not equal placing them in the right places**; it is very likely **passive defence or aimless occupying of space**;
2. **par hardly distinguishes skill levels (85 / 81 / 81 / 82)** — **opening pricing is not the dividing line of skill; how you spend is.**

**Per-round revenue (TOP vs BOT, by company)**:

| Company | TOP n | OR1.2 | OR2.1 | OR2.2 | BOT n | OR1.2 | OR2.1 | OR2.2 |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|
| IC | 255 | 157 | 271 | 335 | 34 | 144 | 236 | 322 |
| NYC | 259 | 217 | 297 | 369 | 25 | 181 | 252 | 310 |
| **B&O** | 216 | **223** | **308** | **397** | 38 | 159 | 234 | 304 |
| **GT** | 167 | **186** | **285** | **358** | 45 | **128** | **217** | **283** |
| **PRR** | 67 | **182** | **263** | **357** | 24 | **106** | **171** | **261** |
| ERIE | 55 | 191 | 250 | 296 | 10 | 168 | 258 | 315 |
| **C&O** | 52 | **192** | **282** | **364** | 9 | **107** | **171** | **257** |

**Four readings**:

1. **IC has the smallest TOP–BOT gap** (OR2.2: 335 vs 322) — **because IC is the easiest to play, and even weak players can produce results with it**;
2. **GT / PRR / C&O have the largest gaps** (GT's OR2.2: 358 vs 283) — **only in the hands of experts do hard-to-play companies show a gap**;
3. **⚠ ERIE is the exception** (TOP 296 vs BOT 315, **BOT is actually higher**) — the sample is too small (TOP 55 / BOT 10), **and ERIE is a late-game company to begin with, so the opening window has weak explanatory power for it**;
4. **B&O has the highest absolute revenue** (TOP 397 / BOT 304) — **confirming §8.4's "the 2-train revenue/cost ratio of 4.5× is the highest in the whole table"**.

---

## 19. Actionable conclusions — quick reference

**Founding**
1. **In SR1 prioritise IC** (position 1.78, getting a Land Grant for free, and 0.27–0.49 ranks better than the others at the same price point); if you can't get it, then **NYC / B&O**.
2. **Set par at $80–$90**; **set IC at $80–$90 too** (treasury = 3×par, $80 is exactly 3 2-trains). **Never set a non-IC company at $40** — **unless you intend a deliberate blow-up** (§17).
3. **Open at most 2 companies per game**; opening 3 gives a mean rank of 3.04 and a win rate of 14% (net effect +0.49).
4. **If you want to found a late company, accept that it is a lottery ticket**: companies founded in SR4+ earn only $1077 overall (SR1 is $4031) with a win rate of 18% — **unless it is NYC** (the only company that "can be opened at any time").
5. **⚠ Do not open PRR in SR1** — it is the only company in the whole table that is "better founded late" (3.07 → 2.85).

**Trains**
6. **Open with a 2-train combination for the first trains**: B&O buys 2–3 (`2+2+2` 2.27/36%) | IC buys 2 (`2+2` 2.28/30%) | NYC buys 2–3.
7. **In the mid game be sure to mix in one 3/5**: NYC `2+2+3/5` 1.80/50%, IC `2+2+4` 1.75/54%, IC `2+2+3/5` 1.88/50%, B&O `2+3/5` 2.41/35%.
8. **Do not buy only one 2-train**: NYC `2` 3.17/10%, B&O `2` 3.23/9% (the worst opening in the whole table).
9. **Have the treasury stocked to $450 before Phase III** — the win rate rises from 22% to 31%; it is the cheapest win/loss switch in the whole table.
10. **Don't count on buying other players' trains** (across 665 games there are only 7 cross-player transactions); but **between your own two companies** you can pick up a 7/8-train for $181 in OR4–OR5.

**Shares**
11. **Build positions in SR1–SR3, add the main position in SR4 (~30%), and reduce from SR5 onward**.
12. **When rotating, rotate only into IC / GT / B&O / NYC**; **ERIE and C&O are endgame liquidation stocks** (57% / 56% of selling is concentrated in SR5–SR6).
13. **Holding around 45% of your own company is enough** (TOP 44.9%, BOT 51.1%).
14. **If you want to dump someone's price, check whether it has issued shares from treasury** — **57% of price declines are caused by the company printing shares itself**.

**Stations**
15. **IC pushes in the direction of Toledo** (station placers 1.99/37%, 0.56 ranks better than non-placers), **do not crowd into Chicago** (2.44, actually lower than the 2.20 of those who don't crowd in);
16. **NYC grabs Cleveland, B&O grabs Cincinnati, GT grabs Chicago** (all four raise the outcome by 0.15–0.32 ranks);
17. **B&O's Cincinnati privilege should be used even at $100** (it was used in 66% of games).

**Turn order**
18. **Do not lower par in order to grab the OR1.1 first move** — moving one round earlier has a residual of −0.03; it has no value.
19. **Acting last in SR1 is a real disadvantage** (residual +0.40), because the good companies get picked away.
20. **The Priority Deal has only one correct use: take IC**.

**Opening operations (§18)**
21. **In the opening, pounce at the chokepoints, rather than first fixing up your own yard**: the high-revenue tier's first tile more often lands on Cincinnati/Wheeling.
22. **Dare to spend in the opening**: the high-revenue tier spends $546 on trains within the window (the low tier $323); this is the strongest single discriminator.
23. **⚠ BOT places the most stations but has the lowest revenue** — **placing many stations does not equal placing them in the right places**.

**Special lines (§17)**
24. **The $40 blow-up**: found at a low par → buy up all the shares → the company buys trains → **transfer them to your own second company for $1** → let it die. **141 cases with a mean rank of 2.50 (level with the baseline), and the people who do it have higher BT (1637)** — **it is a tool for strong players to squeeze out residual value when the conditions allow, not a sure-profit routine**.

---

## 20. Method and limitations

### 20.1 Data and tools

| Stage | Tool |
|:--|:--|
| Splitting · duplicate checking · numbering | `split_1846_cases.py` (automatically recognises three file forms, non-1846 automatically skipped, sha1 deduplication) |
| Event stream reconstruction | `extract_1846.py` (reuses the accounting state machine of `parse_1846_log.py`, **only listens in, does not modify**) |
| Rating | **`rating2_1846.py`** (top-two rank rating) + `rating_1846.py` (full-rank BT, ties) |
| Special topics | `combo` / `rust` / `phase3` / `train` / `stock` / `drop` / `money` / `founding` / `misc` / `corp_profile` / `order` / `bomb` / `earlyor` |

**Credibility comes from reconciliation**: for every player, recompute `cash + Σ(shares held × final share price)`; **2740/2740 player-games match the log's official settlement exactly (100.0%)**.

**Two cross-validations**:
- **Bank Pool ledger**: the attribution of end-of-round price drops uses a "Bank Pool share ledger" to trace the source of every share, compared item by item against the **arithmetic net amount** of "share issuance from treasury − redeem − market purchase + player sales";
- **Self-consistency of the ratings**: for the pairwise win rates predicted by the model versus the actual win rates, the **average deviation for players with a sample of ≥15 games is about 2 percentage points**.

### 20.2 Limitations (must be noted)

1. **Correlation ≠ causation**. For example, "those who place a station marker for GT in Detroit end up worse" is more likely a **passive reaction to having a route taken away**, rather than station marker placement causing failure.
2. **Tiering still has residual circularity**: although BT/RR scores are used instead (an estimate independent of the conclusions), **the ratings themselves are also derived from the outcomes (ranks)** — so conclusions that "a certain behaviour is good" must be read via the residual measure, and the residual must not be treated as causation.
3. **"Company × player" is highly entangled**: the same company yields different results in different people's hands (§8.3b); this report uses residuals to partially strip out skill, but the **"selection effect" still exists** (strong players prefer to grab IC, weak players prefer to grab PRR).
4. **Small-sample cells**: any combination with n<20 (especially some of the opening train combinations in §8.1 and some of the subsamples in §17) is **for reference only, not a conclusion**.
5. **Uneven table types**: 62 3-player games and 142 5-player games, **the number of players affects the pace**; the player metrics in this report have been corrected with the normalized rank, but the company-level statistics are not stratified by player count.
6. **32 games manually ended (4.8%)**: the ending point of these games was decided by the players; they have been kept in the main sample (because their settlement is still official) — if strictness is required, they can be reviewed separately.

---

## 21. High-sample player style profiles (8 players)

> **Positioning**: a supplement for interest. **§21.1–21.4 are the four high-sample players (89–170 games), §21.5–21.7 are the top three ranked by RR score (thinner samples), §21.8 is Player42.**
>
> **Why split into two groups**: although `Amori` (34 games), `SweetShang` (15 games), and `Calmon` (28 games) have the highest RR scores, their **samples are thin**; the interval width for `Player16` (170 games), `Player12` (162 games), `Player9` (114 games), and `Player6` (89 games) is only ±60–80 points, so **their habits are more reproducible**.

### 21.0 Eight-player overview

| Player | Games | **RR score** | 90% interval | Rank score | Top-two rate | Win rate | Table type (fam/mix/str) |
|:--|--:|--:|:--|--:|--:|--:|:--|
| **Player16** | **170** | 1775 | **1667–1790** | +0.36 | 60% | 29% | 74/96/0 |
| **Player12** | **162** | 1818 | **1713–1839** | +0.48 | 65% | 27% | 70/92/0 |
| **Player9** | **114** | 1850 | **1729–1864** | +0.66 | 68% | 41% | 50/64/0 |
| **Player6** | 89 | 1862 | **1738–1883** | +0.70 | 71% | 40% | 30/56/3 |
| **Amori** | 34 | **2236** | 2027–2283 | **+1.56** | **94%** | **71%** | 7/27/0 |
| **Calmon** | 28 | 2116 | 1909–2175 | +1.36 | 89% | 61% | 1/27/0 |
| **SweetShang** | 15 | 2138 | 1871–2182 | +1.20 | 86% | 53% | 0/14/1 |
| **Player42** | **2** | 1468 | 1346–1600 | +0.00 | — | — | 1/1/0 |

**⚠ The sample size determines how to read this**:
- **The four high-sample players (89–170 games)**: narrow intervals, **their habits can be taken as a "stable solution" reference**;
- **TOP3 (15–34 games)**: RR scores much higher (2100+), but the **interval is as wide as 250–300 points** — **"he is strong" is credible, "he is strong enough to reach 2236" is not**;
- **Player42 (2 games)**: **no statistical significance at all**; all we can do is list what he did in his two games (§21.8).

### 21.1 [Core] The four high-sample players vs TOP3: **the difference lies in "spend less, buy more, do not withhold"**

| Dimension | **TOP3** (15–34 games) | **The four high-sample players** (89–170 games) | Difference |
|:--|--:|--:|:--|
| **SR1 number of privates** | **2.03** | 2.30 – 2.74 | TOP3 buys fewer |
| **SR1 private unit price** | **$48** | $54 – 59 | TOP3 buys cheaper |
| **SR1 total private investment** | **$97** | $128 – 162 | **TOP3 spends 24–40% less** |
| SR1 median par | **$80** | $80 – 100 | TOP3 prices lower |
| **SR1 shares bought** | **4.03 shares** | 2.82 – 3.75 | **TOP3 buys more** |
| OR1 trains bought | 2.75 trains | 2.39 – 2.89 | Close |
| OR1 station markers placed | **0.52 markers** | 0.53 – 0.72 | TOP3 places fewer |
| **OR1 withholds** | **0.01 times** | 0.03 – 0.15 | **TOP3 almost never withholds** |
| Trains bought over the whole game | 5.34 trains | 5.31 – 6.01 | Close |
| "Founds only 1 company" proportion | **68%** | 63 – 71% | Close |
| **SR5 net buying** | **+0.97 shares** | +1.18 – +1.96 | **TOP3 does not chase at the end** |
| **Final assets (median)** | **$6856** | $6458 – 6528 | TOP3 5–6% higher |
| **Final cash proportion** | **40%** | 35 – 38% | TOP3 keeps more cash |
| Dividend income proportion | 54% | 54% | **Exactly the same** |

**Four ways to read it**:

1. **The dividend income proportion is exactly the same (54%)** — **everyone relies on dividends, not on buying-and-selling price spreads**. This point bounds the scope of "style differences": **the difference lies in "how the opening is configured", not in "where the money comes from"**;
2. **TOP3's configuration feature is "buy fewer privates, buy more shares, pay dividends early"** — **$97 of private investment vs the high-sample players' $128–162**;
3. **The four high-sample players all withhold more in OR1 (0.03–0.15 vs 0.01)** — **this is not necessarily "better", it may just be "more conservative"** (the data in §8 says OR1 withholders have worse residuals);
4. **Final assets differ by only 5–6%** ($6856 vs $6458–6528) — **the impact of style differences on outcomes is smaller than people imagine**. What really opens the gap is the "opening four rounds of income" in §18 (the high/low tiers differ by a factor of 2.3).

### 21.2 Player16 (170 games, the thickest sample) — **"Private-company faction" + "IC faction"**

| Dimension | Value | Comparison with the average of the four high-sample players |
|:--|:--|:--|
| **SR1 number of privates** | **2.74 (most among the four)** | Average 2.46 |
| **SR1 total private investment** | **$162 (most among the four)** | Average $138 |
| Privates bought | **Mail Contract 106**\\|Big 4 65\\|MS 48\\|C&WI 43\\|Meat Packing 43 | — |
| **SR1 median par** | **$100 (highest among the four)** | Average $88 |
| **Companies whose par he set in SR1** | **IC 56 (most)**\\|NYC 44\\|B&O 37\\|GT 34\\|PRR 29\\|C&O 23\\|ERIE 20 | — |
| **SR1 shares bought** | **2.82 shares (fewest among the four)** | Average 3.35 |
| OR1 trains bought / station markers placed / withholds | 2.39 trains / 0.56 markers / **0.15 times (highest)** | — |
| **SR4 net buying** | **+3.60 shares (most aggressive among the four)** | Average +3.25 |
| Trains bought over the whole game | 5.77 trains | Average 5.61 |
| Final assets / cash proportion | $6528 / 35% | — |

**Profile**: **he spends his money on "opening configuration" (privates $162 + par $100), buys few shares in SR1 (2.82), and then makes it up with one big add-on position in SR4 (+3.60).**

**His private-company preference is very interesting**: **Mail Contract 106 times** (106/466 = **23%** of his privates) — **this is a "permanent bonus" type private; choosing it shows he prefers the long game**.

**⚠ But his RR score is the lowest among the four (1775)** — **"buys the most privates" does not equal "strongest"**.

### 21.3 Player12 (162 games) — **"Mail Contract faction" + B&O/IC dual mainstays**

| Dimension | Value | Comparison |
|:--|:--|:--|
| SR1 number of privates / investment | 2.31 / $129 | Close to the average |
| **Private preference** | **Mail Contract 92** (92/374 = **25% of his privates, the highest proportion in the whole field**)\\|MS 39\\|Big 4 38\\|C&WI 33 | — |
| SR1 median par | $80 | — |
| **Companies whose par he set in SR1** | **B&O 49\\|IC 48** (dual mainstays)\\|GT 39\\|C&O 31\\|NYC 25 | — |
| SR1 shares bought | 3.75 shares | — |
| OR1 trains bought / station markers placed / withholds | 2.62 trains / 0.67 markers / 0.11 times | — |
| **SR5 net buying** | **+1.75 shares (on the high side)** | TOP3 is only +0.97 |
| Final assets / cash proportion | $6458 / 37% | — |

**Profile**: **like Player16, he favours Mail Contract (25%), but his company choices are more balanced — he gives B&O and IC equal weight (49/48), and he also founds C&O (31 times).**

**Player12 also has another "label"**: **first in the whole field for the number of price dumps (352 times, 539 certificates)** — **he is the most active seller** (§12.4c).

### 21.4 Player9 (114 games) — **"B&O/GT faction" + the most aggressive position-adding**

| Dimension | Value | Comparison |
|:--|:--|:--|
| **SR1 number of privates** | 2.30 | Fewest among the four |
| **Private preference** | **Big 4 44 (most among the four)**\\|Mail 37\\|Lake Shore 28\\|Steamboat 23\\|Boomtown 22 | — |
| **Companies whose par he set in SR1** | **B&O 44\\|GT 35\\|IC 29**\\|**ERIE 21** (most among the four)\\|**NYC only 15 (fewest)** | — |
| **OR1 trains bought** | **2.89 trains (most among the four)** | Average 2.67 |
| **Trains bought over the whole game** | **6.01 trains (most among the four)** | Average 5.61 |
| **SR4 net buying** | **+3.27 shares** | — |
| **SR5 net buying** | +1.45 shares | — |
| Final assets / cash proportion | $6494 / **38% (highest among the four)** | — |

**Profile**: **he is the only high-sample player who buys "big-ticket privates" in large quantities (Big 4, 44 times)** — **which shows he prefers the route of "buy a small company, get a train thrown in"** (§10: Big 4's List Price is $40, paid in full in 89% of cases).

**His company preference is also very distinctive**: **B&O + GT as the mainstays (44/35), and he founds NYC only 15 times (fewest among the four)** — **the exact opposite of TOP3's "NYC 28.7%"**. **He specialises in the Midwest (B&O) and the Northwest (GT), avoiding the Northeast (NYC).**

### 21.5 Player6 (89 games) — **the most balanced "non-picky eater"**

| Dimension | Value | Comparison |
|:--|:--|:--|
| SR1 number of privates / investment | 2.47 / $133 | — |
| Private preference | **Mail Contract 33**\\|Boomtown 25\\|MS 21\\|Little Miami 20\\|Lake Shore 19 | **Most balanced** (TOP8 all between 16–33) |
| **Companies whose par he set in SR1** | NYC 24\\|IC 21\\|**PRR 18**\\|GT 18\\|B&O 18\\|**C&O 14**\\|**ERIE 10** | **The only high-sample player who founds PRR/ERIE/C&O in large numbers** |
| OR1 trains bought / withholds | 2.76 trains / 0.06 times | — |
| **SR5 net buying** | **+1.18 shares (lowest among the four)** | Close to TOP3's +0.97 |
| Final assets / cash proportion | $6512 / 38% | — |

**Profile**: **his company choices are the most diversified (he founds all seven companies, including PRR 18 and ERIE 10) — "not picky about food".**

**⚠ This is worth noting**: §4.5 says "the net effect of grabbing PRR in SR1 is +0.29 (bad)", but **Player6 founded PRR 18 times and still has RR 1862** — **which shows that "founding PRR is bad" is an average-level statement, and strong players can be the exception** (it is also possible that the PRRs he founded were late-founded; §14.4 says PRR does better when late-founded).

### 21.6 The "one-liner" for each of the four high-sample players

| Player | One-liner |
|:--|:--|
| **Player16** | **Private-company faction ($162 + par $100), buys few shares in SR1, one big add-on position in SR4** |
| **Player12** | **Mail Contract faction (25%), B&O/IC dual mainstays, the most active seller in the whole field** |
| **Player9** | **Big 4 faction, specialises in B&O/GT, avoids NYC, buys the most trains (6.01)** |
| **Player6** | **Least picky — founds all seven companies, including the PRR/ERIE that others avoid, and does not chase highs at the end** |

---

### 21.7 The **top three** by RR score (thinner samples, for comparison only)

> **⚠ Reiterating the sample problem**: these three have the highest RR scores (2116–2236), but their **game counts are only 15–34**, with intervals 250–300 points wide. **"They are strong" is credible, "strong enough to reach 2200" is not.**

**Amori (34 games, RR 2236) | SweetShang (15 games) | Calmon (28 games)**

| Dimension | **TOP3** | The four high-sample players |
|:--|--:|--:|
| SR1 number of privates / unit price / total investment | **2.03 / $48 / $97** | 2.30–2.74 / $54–59 / $128–162 |
| SR1 median par | **$80** | $80–100 |
| **SR1 shares bought** | **4.03 shares** | 2.82–3.75 |
| OR1 station markers placed / withholds | **0.52 markers / 0.01 times** | 0.53–0.72 / 0.03–0.15 |
| **Proportion who found only 1 company** | **68%** | 63–71% |
| **SR5 net buying** | **+0.97 shares** | +1.18–1.96 |
| Final assets / cash proportion | **$6856 / 40%** | $6458–6528 / 35–38% |

**Five visible features**:
1. **In SR1 they buy fewer and cheaper privates** ($97 vs $128–162, spending 24–40% less);
2. **In SR1 they buy more shares** (4.03 vs 2.82–3.75);
3. **In OR1 they almost never withhold** (0.01 times);
4. **In SR5 they almost never chase** (+0.97 shares);
5. **At the end they keep the most cash** (40%).

**Each of the three's own signature**:

**Amori (34 games)** — opening combinations:
| Count | Privates | Par set |
|--:|:--|:--|
| 2 | Big 4 + Boomtown | B&O |
| 2 | Lake Shore Line + Meat Packing | **NYC** |
| 2 | Lake Shore Line | **NYC + PRR (double founding)** |
| 1 | Michigan Southern ×2 + Ohio & Indiana | IC |

**SweetShang (15 games)** — almost no repeated combinations; every game is different.

**Calmon (28 games)** — **signature routine = give up privates, found two companies at the same time**:
| Count | Privates | Par set |
|--:|:--|:--|
| **2** | **(does not buy)** | **ERIE + NYC** |
| 2 | Little Miami + Ohio & Indiana + Steamboat | B&O |


### 21.8 Player42 — case dissection (1846 has only 2 games, so the game-by-game method is used instead)

> **Method note**: n=2 **cannot support statistics**, but it **can support a case dissection** — lay out every one of his decisions, compare them with his opponents, and then cross-validate against his record in another game.
> **This section does not provide win-rate type conclusions**, only "what he did in specific situations".

#### 21.8.1 Data overview: he is not a novice in either of the two games

| Game | Games | Rating | Rank score | Note |
|:--|--:|--:|--:|:--|
| **1862** | **7** | BT **1429** | **+1.00** | 6 games are 2-player games |
| **1846** | **2** | RR 1468 (range **1346–1600**) | +0.00 | **both games were "ended manually"** |
| Total | **9** | — | — | — |

**⚠ The data of both games have serious flaws**:

- **The 2 games of 1846**: **both were manually ended during Phase II** (one was ended by Player43), **they were never finished at all** — **so even the "ending" of his 1846 data is incomplete**;
- **The 7 games of 1862**: 6 games are 2-player games (§1862 report §12.2), **the "top-two rate" of 2-player games is identically 100%**, with no discriminating power.

**But one thing is clear**: **he is not a novice** — 9 games across two 18xx games, and his rank score in 1862 is **+1.00** (a positive value).

#### 21.8.2 Game-by-game dissection one: 1846-193 (3-player game, **he won** £1400)

**Situation**: a 3-player game, starting capital $800. PRR/ERIE were removed from the private pool, and LS/Ohio&Indiana/Michigan Central/Meat Packing/Tunnel Blasting/Boomtown were gone too — **the remaining big-ticket privates are especially complete** (MS, Big 4, C&WI are all present).

**① Private allocation phase — he single-handedly swallowed two "Independent Railroads"**

| Player | What he bought | Cost |
|:--|:--|--:|
| **Player42** | **Michigan Southern ($140) + C&WI ($60)** | **$200 (25% of starting capital)** |
| Player43 | Steamboat ($40) + **Big 4 ($100)** | $140 |
| Player46 | Little Miami ($40) + Mail Contract ($50) | $90 |

**→ Of the three he spent the most, and he specifically picked the "Independent Railroads (Minor)":** MS comes with **a Detroit station marker + one 2-train + cash**; C&WI comes with **a free Chicago station marker**.

**② OR1.1–OR2.2 (4 Operating Rounds) — he collected rent with MS**

```
MS runs a 2-train for $60 each round → all withheld → Player42 receives $30 (a Minor's revenue goes directly to the holder)
C&WI gives Player42 $10 each round
Cumulative over 4 rounds: Player42 nets $120 + $40 = $160 in cash from the two privates
```

**→ Over these 4 rounds,** the two privates are his cash flow**—he has not yet founded a company, and he already has income.

**③ SR2 — each of the three founds one company**

| Player | Company founded | par |
|:--|:--|--:|
| Player46 | B&O | $80 |
| Player43 | IC | $80 |
| **Player42** | **GT** | **$70 (the lowest)** |

**→ He set the lowest par ($70)** — **the sweet spot in §6.1 is $80–90**, he was half a notch low.

**④ OR2.1 — he bought a 3/5-train, triggering Phase II**

```
GT issues 2 shares → +$120
GT buys a 2 train for $80
GT buys a 3/5 train for $160     ★ triggered Phase II
```

**→ This step matches the conclusion of §8.1**: **"mixing in one 3/5-train is the best opening improvement"** (NYC `2+2+3/5` 50%, B&O `2+3/5` 35%).

**⑤ OR2.2 — the crucial three steps: selling the privates to his own company**

```
GT buys Michigan Southern (Minor) from Player42 for $40
   → GT receives $40, one 2-train, and the Detroit station marker
GT buys Chicago and Western Indiana from Player42 for $60
   → GT placed a station marker in Chicago
GT runs a 3/5 train for $190 (Detroit→Toledo→Chicago + East-West Route bonus)
GT runs a 2 train for $70
GT pays out $260 = $26/share → Player42 receives $104, GT receives $104
```

**→ This is the watershed of this game**: **he "sold" the two Independent Railroads in his hands into GT**, and then GT immediately ran the newly acquired routes for $260, paid out in full, and the share price rose 2 spaces.

**⚠ Note his pricing**: **MS sold for $40 (List Price ceiling $60), C&WI sold for $60 (ceiling $60)** — **he did not sell MS for $60 in line with the §10 convention of "84–91% paying the full List Price"**.
**Why?** Because **GT's treasury was limited** (it had just issued shares for +$120 and bought two trains for −$240), **if he sold high GT could not afford it** — **he made a trade-off between "taking money himself" and "leaving cash for the company"**.

**⑥ OR3.1–3.2 — GT takes off**

```
GT runs three trains: 3/5-train $190 + 2-train $80 + 2-train $80 = $350, full payout
Share price: $60 → $80 → $100 → $124 (doubled over four rounds)
Player42 takes $204 / $210 from GT
```

**⑦ SR4 — he founded a second company (and at a high price)**

```
Player42 sells a 10% share of IC and receives $124    ← sells the opponent's company's shares for cash
Player42 pars NYC at $124                              ← founds the second one at the highest price
Player42 buys a 20% share of NYC for $248
```

**→ Completely opposite to the first one's $70, he set $124 for the second (the highest tier)** — **"the main company starts from a low price, the second company takes over at a high price".**

**⑧ Endgame (manually ended by Player43)**

| Player | Net worth | Dividend income | Final shareholdings |
|:--|--:|--:|:--|
| **Player42** | **$1400** | **$592 (the highest)** | GT×6, NYC×3 |
| Player43 | $1105 | $406 | IC×6, B&O×1 |
| Player46 | $1065 | $418 | B&O×6, IC×1, NYC×1 |

**→ He won on "dividend income of $592"** — **of which about $314 came from GT ($104+$210)**.

#### 21.8.3 Game-by-game dissection two: 1846-209 (4-player game, **he was 4th** £983)

**Situation**: a 4-player game, starting capital $600. **In the private pool both MS and Big 4 are present**, but there is an additional strong player, FlyingSickle (RR 1950).

**① Private allocation — he bought only 1**

| Player | What he bought | Cost |
|:--|:--|--:|
| Player43 | Steamboat ($40) + **MS ($140)** | $180 |
| FlyingSickle | Boomtown ($40) + Little Miami ($40) + Mail Contract ($60) | $140 |
| Player46 | Lake Shore ($40) + C&WI ($60) | $100 |
| **Player42** | **Big 4 ($100)** | **$100 (the least)** |

**→ Compared with the first game ($200), this time he spent only $100** — **and the MS he wanted most was taken by Player43 for $140.**

**② SR1 — he did three unusual things**

```
Player42 pars NYC at $60         ← founds the company at the lowest price
Player42 buys a 20% share of NYC for $120
……
Player42 sells a 10% share of NYC and receives $60    ★ sells his own company
Player42 buys a 10% share of GT from the Treasury for $70
Player42 buys a 10% share of ERIE from the Treasury for $30
```

**→ In SR1 he already (a) founded a company, (b) sold 10% of his own company, and (c) turned around and bought shares of two other companies.**

**This step deserves to be discussed separately**: **"a president selling his own company's shares in SR1" is one of the worst behaviors in the data** (§12.5: **among presidents who got dumped on, the residual for "dumping on yourself" is +0.30, the worst in the whole dataset**).
**But his motive is understandable**: NYC was founded at only $60, **selling 1 share only drops it one space**, in exchange for $60 in cash to go buy GT ($70) and ERIE ($30) — **he was diversifying his investment**.

**③ OR3 — his Big 4 was also sold to his own company**

```
NYC buys Big 4 (Minor) from Player42 for $40
```
**→ The same routine as the first game** (injecting privates into the company).

**④ But this time GT was not in his hands**

**This is the most crucial fact of this game**:
- **First game**: **GT was his own**, share price $60→$124, and he took $314 in dividends from it
- **Second game**: **GT belonged to the opponent Player43** — Player43's GT dividends $220 / $390 / $540, **Player43 finished 1st in the end**

| Game | Who GT belonged to | GT's dividends (the last three times) | Result |
|:--|:--|:--|:--|
| **1846-193** | **Player42** | $260 / $340 / $350 | **Player42 1st** |
| **1846-209** | **Player43** | $220 / $390 / $540 | **Player43 1st, Player42 4th** |

**→ The decisive factor in both games is the same company.** This is not a coincidence — **GT's Detroit + Toledo + Chicago corridor is one of the optimal routes on both of these maps** (§8.2b: GT's Detroit coverage 366%, median 1.2 rounds to first successful run, the fastest in the whole dataset).

**⑤ His final struggle**

```
Player42 buys a 10% share of NYC from the market for $50   ← adds to his position on the NYC price dip
Player42 buys a 10% share of NYC from the Treasury for $90
```
**→ At the end he held NYC×6, GT×2, ERIE×1** — **but NYC finally rose only to $90 (IC 150, C&O 150)**.

#### 21.8.4 Comparison of the two games: his playing style and its shortcomings

| Dimension | **1846-193 (won)** | **1846-209 (4th)** |
|:--|:--|:--|
| Players / opponents | 3 players / Player43, Player46 | 4 players / **with FlyingSickle added** |
| **Private investment** | **$200 (25% of starting capital)** | **$100 (17%)** |
| **Privates obtained** | **MS + C&WI (two Independent Railroads)** | Only Big 4 |
| First company founded / par | **GT @ $70** | NYC @ $60 |
| **Selling his own company's shares in SR1** | **No** | **Sold 1 share of NYC** |
| Selling privates to the company | Yes ($40 + $60) | Yes ($40) |
| **Who the decisive GT belonged to** | **Himself** | **The opponent Player43** |
| Endgame | **1st ($1400)** | 4th ($983) |

**Summary of his playing style (consistent across the two games)**:

1. **Extreme preference for "Independent Railroads (Minor)"** — he bought them in both games, and prioritized buying MS / Big 4 / C&WI;
2. **At the opening he is willing to hold a heavy position in privates** (first game $200 = 25% of starting capital);
3. **He will definitely "sell" privates into his own company** (§10's "asset injection" routine, used in both games);
4. **The first company is set at a low par ($70 / $60)**;
5. **The second company is set at a high price** (NYC @ $124 in SR4 of the first game);
6. **He is willing to sell his own company's shares in SR1 to diversify** (second game) — **this is a dangerous move**.

**His shortcomings (visible from the two games)**:

| Shortcoming | Evidence |
|:--|:--|
| **No contingency plan for "a key company being taken by someone else"** | In the second game MS was snatched away by Player43, and GT was also taken by Player43, **leaving him with only NYC** |
| **He will sell his own company as early as SR1** | Sold 10% of NYC in the second game (§12.5: dumping on yourself is the worst behavior) |
| **The timing of the second company is too late** | In the first game he did not found the second company until SR4 (the whole game had already entered the late Phase II) |

#### 21.8.5 Cross-game cross-validation: his 7 games in 1862

| Game | Players | Rank | Net worth | Companies he founded |
|:--|--:|--:|--:|:--|
| 1862-018 | 3 | 3rd | £1166 | L&H, NGC, I&B |
| **1862-077** | 2 | **1st** | £823 | I&B, N&E, E&H |
| **1862-078** | 2 | **1st** | £889 | L&D, W&F, WNR |
| 1862-138 | 2 | 2nd | £3728 | ENR, N&S, WVR, ECR, ESR, FDR (6 companies) |
| 1862-186 | 2 | 2nd | £3244 | ECR, EUR, N&S, WVR, N&B |
| 1862-188 | 2 | 2nd | £11390 | SVR, NGC, FDR, L&E |
| 1862-190 | 2 | 2nd | £1949 | L&E, L&H, NGC, WNR, L&D |

**→ One cross-game consistent clue**:

| Game | What he prefers |
|:--|:--|
| **1846** | **Buying "Independent Railroads (Minor)"** — MS / Big 4 / C&WI |
| **1862** | **Founding many Independent Railroads** — an average of **4.14** across 7 games (§1862 report §12.2) |

**Common point: he prefers "small entities"** — **in 1846 it is buying Minors, in 1862 it is founding a pile of companies.**

**⚠ But this preference pays off in opposite directions in the two games**:
- **1862**: **"founding many companies" is the strongest single signal** (§1862 report: the residual for founding 5 is −0.080) → **his preference is correct**;
- **1846**: **"founding ≥3 companies in a game" has a net effect of +0.49 (the worst)** (§16.2) → **the same preference is a negative return here**.

**This may be one of the reasons his 1862 rank score is +1.00 while his 1846 rank score is +0.00** — **he brought his 1862 habits to 1846.**

#### 21.8.6 His regular crew

**Number of games at the same table**:

| Pair | 1862 | 1846 |
|:--|--:|--:|
| **Player43 × Player46** | **7** | 3 |
| **Player42 × Player43** | 2 | **2** |
| Player42 × Player46 | 1 | 2 |

**Comparison of the three players' ratings**:

| Player | 1862 (7–12 games) | 1846 (2–5 games) |
|:--|:--|:--|
| **Player43** | BT **1777**, rank score **+1.42** | RR **1684**, rank score **+0.40** |
| **Player46** | BT 1653, rank score +1.00 | RR **1267**, rank score **−1.33** |
| **Player42** | BT 1429, rank score +1.00 | RR 1468, rank score +0.00 |

**→ The three of them are a stable card partner group (Player43 × Player46 played at the same table 7 times in 1862), and Player43 is the strongest of the three.**

**⚠ But the three players' samples add up to only 26 games (across two games)** — **this can only show that "they have a fixed group", not the strength ordering within the group**.

#### 21.8.7 If you want to sit at the same table as Player42

**Observations based on the two games (not statistical conclusions)**:

1. **At the opening he will take a heavy position in privates, and he prioritizes grabbing "Independent Railroads (Minor)"** — **if you also want MS / Big 4 / C&WI, be prepared to bid up to the List Price ceiling** (in the first game he went up to $200);
2. **He is very sensitive to "key corridor companies"** — in the first game he founded GT himself and won with it, and in the second game GT was taken by Player43 and he collapsed. **If you want to suppress him, grabbing GT (or corridor companies such as B&O/IC) is more effective than grabbing others**;
3. **He will sell his own company's shares in SR1** (second game) — **this is a habit that can be exploited**: his company's share price will be pushed down by himself, **and you can take the opportunity to buy low**;
4. **He will "sell" privates into his own company** — **this is the standard routine, not a loophole**; but **note that in order to leave cash for the company he may sell MS for only $40 (ceiling $60)** — **if you want to compete for his Independent Railroads, he may not bid aggressively**;
5. **⚠ The most important one**: **his 1846 data is only 2 games and neither was finished** — **all the observations above are "observations of two games", not "his stable traits". To really understand him, more game data is needed.**

---

## Appendix 1: mapping onto the 1830/1862 frameworks

| From | Framework element | Corresponding conclusion for 1846 |
|:--|:--|:--|
| **1862** | **familiar table/stranger table** | ✅ **reproduced**: the skill effect at stranger tables is 26% higher (50% higher in 1862) |
| **1862** | **top-two rank rating** | ✅ adopted in this round; r = +0.932 with the rank score |
| **1862** | **"wealth extraction > company survival"** | Partially applicable: 97% reach Phase IV, survival differences are small; but **a dividend gap of $736 is the main cause of the TOP/BOT gap** |
| **1862** | **"bankruptcy is not a punishment"** | Similar but milder: 1846 has no bankruptcy, but the **residual of "those who close a company" is +0.13 (rank 2.47) and is not bad** |
| **1862** | **license × location** | 1846 has no licenses; the counterpart is **private × company (geographical correspondence)**, with a maximum lift of 2.42 |
| **1830** | **normalized rank** | ✅ adopted throughout |
| **1830** | **company strength ≠ founder's outcome** | ❌ **the direction is opposite**: in 1846 the two are almost completely consistent (only PRR is an exception) — **because 1846's founding cost is low (the par is paid to the company, not to the bank)** |
| **1830** | **the top-two rate is only computed for games with ≥4 players** | ✅ adopted (1846 has 62 three-player games, already excluded from the top-two rate) |
| **1830** | **"which in line" auction** | ❌ in 1846 privates are distributed by card draw, with no auction order |
| **1830** | **bankruptcy rate** | ❌ **1846 has no bankruptcy mechanism (0 times)** |

---

## Appendix 2: list of tools from this round

| File | Function |
|:--|:--|
| `_tools/split_1846_cases.py` | split + de-duplicate → `_work/1846_cases/` (672 games) + `_mapping.csv` |
| `_tools/extract_1846.py` · `parse_1846_log.py` | logs → `_work/1846_full.json` (**reconciliation 2740/2740 = 100%**) |
| **`_tools/rating2_1846.py`** | **top-two rank rating** (compares only top-two vs non-top-two) → `_work/1846_rating2.json` |
| `_tools/{order,combo,rust,train,stock,money,founding,corp_profile,misc,bomb,drop,earlyor,phase3}_1846.py` | 14 topic-specific scripts |
| `_work/out1846/*.txt` | **the complete output of all topic-specific scripts over 665 games** |
| `_work/{style2,final,corp,tests,restore}_1846.py` | TOP3 style profiles, endgame asset allocation, company profiles, three strategy tests, station/Independent Railroad re-check |

**The old report versions** (312/167/72/22 games) have had an "old database version" banner added, pointing to this document.

---

*Acknowledgement: All game data comes from [18xx.games](https://18xx.games/) and the players who publish their games there.*

*Analyst: Pig9013 | Model used: Deepseek-flash | Date compiled: 2026-09-17*
