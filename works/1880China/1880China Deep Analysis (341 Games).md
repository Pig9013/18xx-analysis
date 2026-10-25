# 1880 China Deep Analysis (341 Games) · v1.3

> **Analyst**: **Pig9013** | **Model used**: **Deepseek-flash** | **Date compiled**: **2026-09-22**
> **Acknowledgement**: All game data comes from **[18xx.games](https://18xx.games/)** and the players who play there — it is their freely published play records that made this analysis possible.
>
> **Version history**
> - **v1.0 (2026-09-22)**: first release. Replaces the 2026-09-17 draft *1880China Deep Analysis (358 Games)* (that draft had only §0 data inventory and §2 rules notes, and **no game statistics whatsoever**). Completed: 100% reconciliation, player tiers, turn order and auctions, Foreign Investors, company founding, cash flow, era-by-era behaviour, TOP play, §0–§10 in total.
> - **v1.1 (2026-09-22)**: **five metric corrections and additions** —
>   ① **the bid metric was corrected** to "**winning bid − minimum bid (face value)**" (user-specified); §3.2/§3.3 were rewritten accordingly: **= 0 in only 33 auctions (1.2%), all of them in cold auctions with a single bidder**, median +25;
>   ② added **§3.7 "bid band → buyer outcome" for each Private Company**;
>   ③ added **§4.6 the investor sequence (all privates auctioned first, then the draft — 341/341) and the "spending rank → turn order → investor" pairing chain**;
>   ④ added **§5.7 change of Director** (138 instances, 79.7% sold by the incumbent himself), **§6.5 personal loans** (loan-takers' top-two rate 19.8%), **§7.7 station placement** (Tianjin 60.3%, L14 60.0% best);
>   ⑤ the log-trap table grew from 12 to **13 rows** (new: "ghost bids on a closed auction").
> - **v1.2 (2026-09-22)**: **four additions and metric corrections** —
>   ① added **§3.8 opening bids** (opening at face value 54%–76%; opening = face value and final = face value in only 0%–4.5%; a "jump bid" suppresses further raises but pushes the winning bid higher);
>   ② added **§3.9 Private Company pairings** (Private × Major Company, Private × Foreign Investor, both with **lift**);
>   ③ **§7.1 / §7.4 were switched to a "company-Operating-Round" normalized basis** (removing the artefact that later eras have more companies): the train-purchase rate in era D falls to 22% of era A, and **the contribution-rate peak is at OR9 (0.061 per round), not OR10**;
>   ④ **§7.6.1 attribution of train transfers**: four hypotheses tested one by one — **"cash squeeze" holds (64.7% of transfers happen when the buyer cannot afford the new train)**, while the "multiple companies" and "accelerating eras" hypotheses are **refuted by the data**.
> - **Versioning rule (applies from v1.1 onward)**: **every change must bump the version number and state here what changed**; before editing, save a full snapshot to `_work/1880_report/报告_vX.Y_YYYYMMDD.md`.
> - **v1.3 (2026-09-24)**: **player anonymisation** — the TOP group (the 8 in §1.5: djanyreason, cougar, nestorjj, Milu42, Stephen_Yu, plinkb, Fohlenfab, Ken Boucher) keeps real names; **all MID/BOT players are rendered as "PlayerX"** (numbered by BT descending, MID band first, then BOT). Same batch: **the mid/bot headcounts in §1.5 and §8.1 were swapped** (originally written mid 15 / bot 13; actually **mid 13 / bot 15** — the player-game figures 231/195 were correct); the stray `**` left in the §2.3 phase table's header row (a formatting defect) has been cleared; and the two sentence-trap rows in §0.3 were rewritten with anonymised handles. **The same batch produced the English edition `1880China Deep Analysis (341 Games).md`** (a complete Chinese-to-English rendering, sentence for sentence, with matching numbering).
>
> **Chinese edition**: **`1880China深度分析 (341局).md`** — the Chinese original of this report, translated into English on 2026-09-24. This is a **literal, complete** rendering: same headings, same tables, same numbers, and **player handles correspond one-to-one** (`Player7` in English = `玩家7` in the Chinese text; TOP-group names are unchanged). Verified mechanically: the heading hierarchy, the table block sizes and the numeric multiset all match the source exactly.
>
> **Data basis**: `复盘分析/1880China/` — **6 source files** (32.1 MB) → 384 games split out → **358 unique** after de-duplication
> **Main statistical sample**: **341 games / 1,484 player-games / 517 players** (17 "Game ended manually" poll-terminated games excluded — those never reached the endgame, so net worth is not comparable)
> **Player-count distribution**: 3-player 55 | **4-player 160** | 5-player 94 | 6-player 15 | 7-player 17 (**no 2-player variant**; this title supports 3–7)
> **Reconciliation**: **1,564/1,564 player-games = 100.00% exact match** (residual 0; basis in §0.2)
> **Rules basis**: Lookout GmbH 2022 English rulebook (24 pages, i.e. `英规1880chinaRules.pdf`), checked clause by clause; the full Chinese translation is `1880China_规则中文翻译.md`

---

## Table of Contents

| Part | Section | Content |
|:--|:--|:--|
| Data | §0 | Attribute cross-section (**this title has no random attributes**), 100% reconciliation, **13 log traps**, game types, baselines, **a check of the Chinese translation's mistranslations (including records overturned)** |
| Players | §1 | top-two rank rating + BT dual metrics, **top/mid/bot tiers**, familiar table vs stranger table |
| Mechanics | §2 | phase table, Building Permits, Foreign Investors, Communist Takeover; **a differences table against 1830／1846／1862** |
| **Turn order** | §3 | **provisional order → auction → Private Companies**: **full table of bid premiums (winning bid − face value)**, **= 0 in only 33 auctions (1.2%)**, **opening bids (opening at face value 54%–76%)**, **bid band → buyer outcome**, **Private × company pairings (lift)**, "the more Privates the better" |
| **Pairings** | §4 | **Foreign Investors**: **sequence (auction first, then draft — 341/341)**, **the chain from spending rank to turn order**, **the three-way Private × company × investor pairing**, French Investor → BCR 243 times, P6 → BCR with zero exceptions |
| **Founding** | §5 | appearance rate, founding timing, par price, **full SR1 founding table with win rates**, Director's holdings after founding, **138 changes of Director (79.7% sold by the incumbent)** |
| Cash | §6 | the era-by-era evolution of players' cash, company treasuries, dividend vs withhold, contributions and **personal loans (loan-takers' top-two rate only 19.8%)** |
| Eras | §7 | behaviour by era on a **"company-round" normalized basis**, revenue per train, **train calendar**, **crisis calendar (peak OR9)**, **8.6 transfers per game and four attributions**, **station placement and outcomes** |
| Play | §8 | top/mid/bot behaviour comparison, **top's three replicable traits**, the highest-win-rate play |
| Quick ref | §9 | a table-side checklist you can memorise |
| Method | §10 | toolchain, cross-validation, six limitations |

---

## 0. Data and Reconciliation

### 0.1 Attribute cross-section: **this title has no random attributes**

Before analysing a title, ask: **which attributes are re-bound at random every game?** In 1862 the 16 operating permits are bound to companies at random each game, so "company X is strong" is a meaningless proposition and everything has to be broken down to (company × permit). **1880 is not like that**:

| Attribute | Random? | Note |
|:--|:--|:--|
| Private Companies P0–P7 — price / income / special ability | **Fixed** | all 8 are present every game (rules §VI) |
| Foreign Investors A1–A7 — nationality / Home Base | **Fixed** | A1 Russia, A2 Dalian, A3 Taiyuan … A7 French Indochina |
| The 14 Major Companies — Home Base / colour | **Fixed** | e.g. HKR = Shanghai, BCR = Chengdu (rules §VIII) |
| The 14 Major Companies — abbreviations | **Fixed** | JHA／JHU／JGG／JLR／BZU／LHR／SCR／CKR／BCR／NXR／HKR／NJR／QSR／WNR |
| Players' seating each game | Random | drawing number cards (0–6) sets the **provisional order** |
| The Private Company auction result each game | Player-determined | — |
| The letter combination of Building Permits (ABC／BCD…) | Player choice | the length of validity is set by the size of the Director's Certificate |

> **Conclusion: 1880's company-strength conclusions carry over across games** (the exact opposite of 1862). §5's "which company is worth founding" is therefore an **actionable conclusion**, not statistical noise.

### 0.2 Reconciliation: 341 games, 1,484 player-games, **100.00% exact match**

The last line of the log is the official settlement sheet, of the form
`-- Game over: Fohlenfab (¥7212), nestorjj (¥6886), Player2 (¥6824), Player25 (¥6425) --`, listing each player's final net worth in rank order.

The bookkeeping identities (`_tools/parse_1880_log.py`):

```
player cash = starting capital (3→600/4→480/5→400/6→340/7→300)
            − Private Company auction bids
            + Private Company fixed income ("collects ¥N from Px" at the start of each OR)
            + P0 one-off payment (¥40/70/100, when buying the last 2+2／3／3+3)
            + dividends (the log lists "¥N to X" per person, booked per person)
            + ¥50 connection bonus (when Foreign Investors merge)
            + investor treasury transfers ("transfers ¥N to player" when the 20% option is chosen)
            − share purchases (all "from the IPO"; the special route for the BCR Director's Certificate is §0.3-⑧)
            + share sales (net of brokerage)
            + loan principal − loan repayment
            − Director contributions for a Forced Train Purchase ("contributes ¥N")

net worth = cash + Σ(stake % × final share price) ÷ 10 − debt (including 50% interest)
```

| Item | Result |
|:--|:--|
| Player-game reconciliation | **1564 / 1564 = 100.00%** |
| Residual | **0** (not one cent of deviation; there is no "median residual" to speak of) |
| Game coverage | all 358 / 358 games completed, no parse exceptions |

### 0.3 Log sentence traps (**13 measured, every one of them hit**)

| # | Trap | Consequence | Fix |
|:--|:--|:--|:--|
| ① | Player names contain punctuation (**the original names contain `.`, `()` and the like**; now recorded as `Player20`, `Player23`, `Player22`) | `\b` boundary matching misses the whole name → all of that player's actions are lost (once caused 40 of 517 players to be mis-booked) | change the boundary to `(?=\s\|'s\|$)` |
| ② | Names in the dividend payee list **carry a trailing space** (`¥104 to Player7 `) | the payee fails to match → that dividend vanishes | `strip()` each segment before matching |
| ③ | The dividend payee list **cannot be split on `[^,)]`**: some player names **originally carry a `()` suffix** (now recorded as `Player23`) | that player's dividends for the whole game are lost (once caused a −5707 residual for one person) | split on `', '` and then parse |
| ④ | The settlement sheet allows **negative net worth**: `Player19 (¥-31)` | the regex `¥([\d,]+)` does not match the minus sign → that player is dropped for the whole game | `¥(-?[\d,]+)` |
| ⑤ | Numeric actors: in operations a Foreign Investor is referred to by **number** (`6 runs a 2 train`), but in merges/the draft by **name** (`French Investor and BCR are connected`) | investor income/expense mis-booked | two id maps |
| ⑥ | `X sells **3 shares** of SCR` (plural) coexists with `X sells a **30%** share` | sales missed → cash overstated | write both sentence forms |
| ⑦ | `increases their loan by 50% (¥4) to ¥X` — a literal parenthesis in the regex adds an extra capture group | `¥4` gets parsed as a number and **the whole game crashes** | use `\(%s\)`, not `\((%s)\)` |
| ⑧ | Obtaining the BCR Director's Certificate from P6 goes through `X receives a 20% share of BCR`, **with no amount written**; and when the buyer himself holds P6 **no money moves at all** | ¥200 wrongly deducted → a −200 residual for one person | branch explicitly on buyer and seller |
| ⑨ | `Five shares of X have been bought. X receives ¥N` is a **standalone line with no actor** | the company's second capital instalment is missed → treasury too low, contributions overstated by 31 | match the standalone line first |
| ⑩ | Investor transfers have two subject forms: `French Investor transfers ¥N to CKR` and, at settlement, `5 transfers ¥92 to Player7` | end-of-game investor dividends missed | the numeric branch must handle transfers too |
| ⑪ | `Priority order: a, b, c, d` and `X has priority deal` are the **authority on order**, but carry no prefix | seating could only be guessed otherwise | extract explicitly (the whole turn-order metric of this report uses it) |
| ⑫ | The `-- Game over:` line also starts with `--` | it gets swallowed as a round-marker line | special-case it in the marker parser |
| ⑬ | **Closed auctions still carry the occasional "ghost bid"** (`1880-018`, `1880-058`, 1–2 lines each, where a player bids again on a Private Company already sold) | it splits the same Private Company into two auction blocks, so the bid count inside each block is too low | take "who won + winning bid" as the authority; the bid sequence is reference only |

### 0.4 Game-type composition and baseline numbers

| Item | Value |
|:--|:--|
| Games | 341 (normally finished) |
| Player-games | 1,484 |
| Players | 517 (of whom **36 have n≥8 games**) |
| Player-count distribution | 3-player 55 (16.1%) | **4-player 160 (46.9%)** | 5-player 94 (27.6%) | 6-player 15 (4.4%) | 7-player 17 (5.0%) |
| Final phase | **D2 in 174 games + D3 in 167 games = 100%** — this title **always runs to the second 8-train** before ending; there is no early bankruptcy termination |
| Number of ORs | **always exactly 10** (range 10–10) — the end condition is the fixed "every company operates 3 more times after the last 8-train" |
| Major Companies floated per game | average **11.87 / 14** |
| Private Companies | **all 8 sold, 100%** (341/341, no failed auctions) |

**Baselines (used for comparison throughout)**

| Metric | All |
|:--|:--|
| Top-two rate (≥4-player basis) | **46.0%** |
| Win rate | 23.0% |
| Mean normalized rank | 0.500 (= chance level) |
| Final net worth | median **¥6643**, mean ¥6793, min ¥−1043, max ¥15690 |
| 1st place net worth | median ¥8105 |
| Gap between 1st and 2nd | median **only ¥521** (mean ¥736) |

> **The first counter-intuitive number**: the median gap between winner and runner-up in this title is only **¥521** — about the final price of a 5% stake in one company. **1880 is an extremely tight game: a "one step from a comeback" situation that is rare in 18xx.**

### 0.5 Checking the Chinese translations for mistranslations (**each item re-verified this round**)

The English rules are the sole authority. The actual problems in the two Chinese translations (**each item was re-grepped/re-compared on 2026-09-22**; full detail in appendix B of `1880China_规则中文翻译.md`):

| # | Translation | Problem | Verdict |
|:--|:--|:--|:--|
| 1 | **Official bilingual edition** (`en_1880china…zh-CN.pdf`) | under the "Restored 2-train" entry it merges the English *are available after the first 6-train is bought* with *do not rust* into one sentence, **inserting "Type E" out of thin air** | ✅ **a substantive mistranslation** — it turns the **2R (Restored 2-train)** into a Type E train, which has nothing to do with it |
| 2 | Official bilingual edition | the word Tile has **four different renderings coexisting**: 版块 63 times, 板块 27 times, 地形牌 2 times, 图块 2 times | terminology inconsistency (within one file) |
| 3 | Official bilingual edition | Rust → 锈蚀 (2 places); *do not fulfil the obligation to own a train* → "不履行拥有列车的义务" | ✅ **translated correctly** — **overturns two earlier records** ("Rust rendered as 铁路" and "that obligation sentence reversed" are both false in the official bilingual edition) |
| 4 | **Community translation** (`1880中国铁路中规 pp注释版`, translated by 赤发灵官) | Treasury → "**财物部**" (only 1 place in the whole text; the other 4 read "金库") | mistranslation |
| 5 | Same | withhold → "**隐瞒**" (should be **留存**) | wrong word (隐瞒 carries a connotation of fraud) |
| 6 | Same | in the share-price-table bonus example **¥50 / ¥5 / ¥120 are all translated as "英镑" (pounds)** | wrong currency unit |
| 7 | Same | *do not fulfil the obligation to own a train* → "**不受拥有火车的数量限制**" | **semantic inversion** (the correct meaning is "does not fulfil the obligation to own a train") |
| 8 | Same | the whole Restored 2-trains entry **left untranslated in English** | omission |
| 9 | Same | tiles rendered as "轨道片" (76 places), ferry/ferry-boat/ferry mixed, station marker/station sign/station symbol mixed | terminology inconsistency |

> ⚠️ **This also corrects the project's own glossary**: the entry in `18XX_中英术语对照表.md` §5.3 that "Director is translated as "总监" (1880 pp annotated edition)" **does not hold up** — in that draft's text layer "总监" appears **0 times** and "董事" 58 times. That entry has been withdrawn from the glossary (if the original note came from an image and never entered the text layer, it needs separate explanation).

---

## 1. The Player Layer

### 1.1 Basis: why top-two rank rating, not average rank

- **Average rank is not comparable across player counts**: the theoretical mean is 2.0 in a 3-player game, 2.5 in a 4-player game and 3.0 in a 5-player game. This title mixes 6 different player counts, and average rank systematically flatters "people who often play 3-player games".
- **Normalized rank =(rank−1)/(players−1)** solves the cross-count problem, but it **throws away the "top two" distinction that is worth the most in this title** — this title's payoff curve drops steeply after second place (1st ¥8105, 2nd ¥7438, 3rd ¥6713, see §0.4), and **rank is the score** (final net worth): one place is an order of magnitude.
- So the primary metric is the **top-two rank rating (RR)**: 1st **+2**, 2nd **+1**, 3rd onward **−(rank−2)**; the model constructs only "**top two vs not top two**" comparison pairs — 1st is not compared with 2nd, and 3rd is not compared with 4th.
- The parallel metric is **Bradley-Terry** (L2-regularized logistic regression on log-strength, regularization λ=1.0), used for cross-validation and tiering.

### 1.2 Three base metrics

| Metric | Definition | Used for |
|:--|:--|:--|
| **RR/game** | (2·wins + 1·seconds − Σ(from 3rd place on (rank−2))) ÷ games | the main column of the strength table |
| **Top-two rate** | games finished in the top two ÷ games | win-rate comparison after tiering |
| **Normalized rank** | (rank−1)/(players−1), 0 = win, 1 = last | cross-player-count comparison |
| **BT×1000** | strength score normalized so the geometric mean is 1.0 | the basis for tiering |

> **On the sample basis for the top-two rate (required reading)**: in a 3-player game the top-two rate is always 66.7% (2 of 3 players are in the top two), which has **no discriminating power**, so **the strict basis for the top-two rate is "computed on ≥4-player games only"**. Most tables in this report use the **full sample (3–7 players)** to preserve sample size, but **every conclusion has been re-computed and checked on ≥4-player games: the ordering is identical and the values are slightly lower**. The two sets of numbers for the four headline conclusions:

| Conclusion | Full sample | **≥4-player basis** |
|:--|:--|:--|
| Number of Privates (4 vs 0) | 55.8% vs 41.7% | **49.0% (n=49) vs 39.3% (n=107)** |
| Number of companies directed (4 vs 1) | 61.8% vs 18.4% | **58.4% (n=154) vs 18.6% (n=113)** |
| Founding HKR in SR1 | 59.9% | **55.5% (n=254)** |
| Portuguese vs Japanese investor | 54.6% vs 31.1% | **52.3% vs 31.4%** |

### 1.3 Strength table (**the 36 players with n≥8 games**, sorted by BT)

| Player | Games | Wins | Top-two rate | Normalized rank | RR/game | BT×1000 |
|:--|--:|--:|--:|--:|--:|--:|
| **djanyreason** | 27 | 13 | **92.6%** | 0.157 | **1.33** | **8330** |
| **cougar** | 45 | 30 | **86.7%** | 0.130 | **1.36** | **5290** |
| **nestorjj** | 24 | 10 | **83.3%** | 0.206 | 1.04 | **4120** |
| Milu42 | 10 | 5 | 90.0% | 0.153 | 1.10 | 3680 |
| **Stephen_Yu** | 23 | 10 | 73.9% | 0.250 | 0.61 | **2630** |
| plinkb | 18 | 6 | 66.7% | 0.312 | 0.50 | 2580 |
| **Fohlenfab** | 44 | 19 | 70.5% | 0.269 | 0.61 | **2480** |
| Ken Boucher | 9 | 5 | 100% | 0.194 | 1.56 | 2340 |
| Player1 | 17 | 3 | 64.7% | 0.348 | 0.29 | 1900 |
| Player2 | 18 | 1 | 66.7% | 0.417 | 0.33 | 1720 |
| Player3 | 15 | 4 | 66.7% | 0.352 | 0.47 | 1570 |
| Player4 | 16 | 5 | 62.5% | 0.401 | 0.31 | 1570 |
| Player5 | 9 | 4 | 77.8% | 0.343 | 0.67 | 1560 |
| Player6 | 13 | 9 | 84.6% | 0.154 | 1.38 | 1500 |
| Player7 | 23 | 6 | 60.9% | 0.354 | 0.26 | 1000 |
| Player8 | 56 | 13 | 42.9% | 0.403 | −0.29 | 900 |
| Player9 | 20 | 1 | 45.0% | 0.471 | −0.50 | 900 |
| Player10 | 9 | 4 | 77.8% | 0.315 | 1.00 | 850 |
| Player11 | 17 | 7 | 64.7% | 0.328 | 0.53 | 840 |
| Player12 | 10 | 2 | 50.0% | 0.435 | −0.40 | 720 |
| Player13 | 8 | 2 | 62.5% | 0.375 | 0.50 | 700 |
| Player14 | 11 | 5 | 63.6% | 0.341 | 0.55 | 690 |
| Player15 | 17 | 5 | 35.3% | 0.505 | −0.47 | 650 |
| Player16 | 25 | 5 | 68.0% | 0.513 | 0.48 | 620 |
| Player17 | 14 | 1 | 28.6% | 0.518 | −1.14 | 500 |
| Player18 | 14 | 3 | 57.1% | 0.560 | 0.07 | 500 |
| (the remaining 10 omitted; all 517 in `_work/1880_rating.json`) | | | | | | |

> **Sample warning (required reading)**: **cells with n<20 are indicative only**. In the table above, Ken Boucher (9 games), Player10 (9 games), Milu42 (10 games) and others have BT confidence intervals wider than ±2000 points, and **cannot be equated with cougar (45 games) or Player8 (56 games)**. Of this title's 517 players, **448 have fewer than 5 games** — they are "passers-by", entering the BT computation only as opponent strength, and take no part in behavioural tiering.

### 1.4 Consistency of the two metrics

The rank correlation between RR/game and BT is **ρ = 0.77** (Spearman, n=36, those with n≥8 games) — **the direction agrees but the ordering differs materially**, which is exactly why BT is used for tiering. The two most obvious divergences:

- **Ken Boucher**: RR 1.56, **the highest in the whole set** (5 wins and 4 seconds in 9 games), but BT only 2345 (8th) — his opponents are on the weak side overall, and BT has discounted that by opponent strength.
- **Player8**: 56 games, RR −0.29 (a negative score), yet BT 903 (around the median) — his win rate is below 50%, but he **plays at the same table as strong players year in, year out**, and BT gives him the position he deserves.

**Tiering always uses BT** (opponent strength is already corrected for).

### 1.5 Tiers: top / mid / bot

| Tier | Cut-off | Players | Player-games to analyse | Note |
|:--|:--|--:|--:|:--|
| **top** | BT ≥ 2.0 and games ≥8 | **8** | 200 (600 after weighting) | djanyreason, cougar, nestorjj, Milu42, Stephen_Yu, plinkb, Fohlenfab, Ken Boucher |
| **mid** | 0.7 ≤ BT < 2.0 | 13 | 231 (462 weighted) | Player1, Player2, Player3… |
| **bot** | BT < 0.7 | 15 | 195 (195 weighted) | Player14, Player15, Player16… |
| small | games < 8 | 481 | not tiered | opponent strength only |

**Weighting basis**: when comparing group behaviour, weight **top 3 / mid 2 / bot 1** (top's voice is the loudest), and convert every metric to a "per player-game mean" before comparing. **Note: the weighting is used only to describe "how high-level players play", never for causation — see §10 limitation ③.**

### 1.6 Familiar tables vs stranger tables

**Criterion** (following the 1846 report's basis): if in a game **every pair** of players had met ≥2 times before → familiar table; otherwise a stranger table.

| Type | Games | Mean players | Top-two rate (≥4-player) | top-tier top-two rate | bot-tier top-two rate |
|:--|--:|--:|--:|--:|--:|
| **Familiar** | 56 | 3.84 | 48.2% | 3/3 = 100% (**n=3, meaningless**) | 15/32 = 46.9% |
| **Stranger** | 302 | 4.47 | 42.6% | 146/186 = **78.5%** | 40/131 = **30.5%** |

**Conclusion**: this title's **familiar-table sample is too small (56 games, of which only 42 are ≥4-player)** to support a tier comparison — the top tier in the familiar row above has only 3 player-games. **Only two things can be said**: ① the mean player count in familiar games is clearly smaller (3.84 vs 4.47), meaning acquaintances play more 3-player games; ② in stranger games the top-two rate gap between top and bot is **48 percentage points** (78.5% vs 30.5%), so **skill explains a great deal in stranger games** — consistent in direction with the 1846 conclusion (1846 stranger |r|=0.584 > familiar 0.463). Whether familiar tables compress the skill gap **this title's data cannot answer; do not draw a conclusion**.

---

## 2. Mechanical Facts (state the rules clearly first, then discuss strategy)

> This section covers rules only and **gives no strategic advice**. Clause numbers correspond to `英规1880chinaRules.pdf` (for the Chinese translation see `1880China_规则中文翻译.md`).

### 2.1 The three-layer structure: Private Company → Foreign Investor → Major Company

| Layer | Number | How obtained | Effect |
|:--|:--|:--|:--|
| **Private Companies P0–P7** | 8, all present every game | **Auction** (§V, ascending P0→P7, bids must be a multiple of 5) | Fixed income each OR (P0–P5) + **special abilities** (P0 one-off payment, P2 free ferry, P3 Taiwan +20, P4 river track-laying −¥20, P5 a Phase D Building Permit, P6 gives a BCR Director's Certificate, P7 Rocket of China exchanged for a train) |
| **Foreign Investors A1–A7** | **N** are chosen (N = player count), the rest are removed from the game | **Free draft** (§VII), picked in the new seating order | Have a Home Base (Russia／Dalian／Taiyuan／Qingdao／Hong Kong／Macau／French Indochina), **rent** currently available trains, always lay/upgrade only 1 tile, need no Building Permit, **have no share price**; income stays in their own treasury |
| **Major Companies** | 14 | buy a Director's Certificate and reach the "prescribed number of shares" to float (§XII) | ordinary 18xx companies |

**The merger of Foreign Investors (this title's only merger mechanism, §VII)**: at the end of each investor turn, check the **combined route network** — "can a single train of unlimited length run from one side's home station to the other side's home station". Once connected: the investor merges into the company and the **treasury is transferred to the company in full**, or the **owner takes 20%** (the other 80% goes to the bank); the owner additionally receives a **¥50 connection bonus** + the investor's reserved 10% share.

### 2.2 Building Permits — **this title's most unique mechanism**

| Point | Rule (§VIII) |
|:--|:--|
| Effect | **without a Building Permit valid for the current phase, a company cannot lay track/upgrade tiles** |
| Validity is determined by the size of the Director's Certificate | **40% → 1 phase; 30% → 2; 20% → 3** |
| Choice | chosen freely when buying the Director's Certificate, and **must be consecutive** (ABC or BCD, not ABD) |
| Exception | Foreign Investors need no Building Permit |
| Phase | the four seasons A/B/C/D (P5 provides one extra permit for Phase D) |

> **This is the most fundamental difference between 1880 and 1830／1846／1862**: the right to lay track is not automatic — it is a **"phase package" bought outright when buying the certificate**. The smaller the Director's Certificate (20%) → the longer the permit (3 phases) → but the smaller the stake and the slower the start; the larger the certificate (40%) → it covers only 1 phase, but **floats immediately by meeting the threshold**.

### 2.3 Phases and Trains (§XI)

| Phase | Train type | Price | Number | Rusts | Available tiles | Train limit | Key |
|:--|:--|--:|--:|:--|:--|--:|:--|
| A1 | 2 | 100 | 10 | | Yellow | 4 | a company floats at **20%**; capitalisation 50%; only 5 shares available |
| A2 | 2+2 | 180 | 5 | | Yellow | | |
| B1 | 3 | 180 | 5 | | +Green | | a company floats at **30%**; all 10 shares available; **may buy a train from a company with the same Director**; a company that is 50% sold receives a second instalment of capital |
| B2 | 3+3 | 300 | 5 | | +Green | 3 | |
| B3 | 4 | 300 | 5 | rusts 2 | +Green | | a company floats at **40%**; **the Communist Takeover begins**; P7 forced exchange (the second 4-train) |
| C1 | 4+4 | 450 | 5 | rusts 2+2 | +Brown | | brown tiles available |
| C2 | 6 | 600 | 5 | rusts 3 | +Brown | | a company floats at **60%**; **2R trains available (¥250)**; **the Shanghai Stock Exchange reopens** (the Communist Takeover ends) |
| C3 | 6E | 700 | 5 | rusts 3+3 | +Brown | 2 | |
| D1 | 8 | 800 | 2 | rusts 4 | +Grey | | grey tiles; **station marker prices double**; after the second 8-train every company operates 3 more times |
| D2 | 8E | 900 | 2 | rusts 4+4 | +Grey | | |
| D3 | 10 | 1000 | * | | +Grey | | |

**Revenue and bonuses (§X)**: route revenue + **share-price-table bonus** (the bonus value of the space the marker is on ×10) + **Trans-Siberian Railway bonus** (running a train A3↔A15 +¥50) − **ferry fee** (¥10 each time it is used, at most 3 times).

### 2.4 The Communist Takeover (§XIV, 1949–1990)

Between **the sale of the first 4-train → the sale of the first 6-train**: share prices are **frozen** (they do not move in either the Stock Round or the Operating Round), Directors may not sell their own shares, Private Companies stop paying dividends (but **their abilities remain**), Foreign Investors no longer operate (**mergers still happen**).

### 2.5 Differences table against 1830／1846／1862 (**to prevent confusion; for reference when translating or checking rules**)

| Dimension | 1830 | 1846 | 1862 | **1880** |
|:--|:--|:--|:--|:--|
| Track lays per round | 1 | 2 | per permit | **1 (BCR 2); from Phase B on: 2 yellow tiles or 1 upgrade — and all of it is constrained by Building Permits** |
| Leader title | president = President | president = President | president | **director = Director** (76 occurrences of director in the whole text, 0 of president) |
| Bankruptcy | yes (23.3%) | no | yes (61%) | **No. Players never go bankrupt**; insufficient cash → **director contribution**, still insufficient → a **personal loan** at 50% interest |
| Share pool | Bank Pool | Bank Pool | Bank Pool | **no Bank Pool** (§XII: shares that are sold go straight back to the bank) |
| Random attributes | none | none | **the 16 permits are bound to companies at random each game** | **None. All fixed** (§0.1) |
| Number of stations | printed on the company mat | 3–5 | | **3 per company** (home station + 2); extra stations ¥40/¥100, rising to ¥80/¥200 in Phase D |
| Merger/takeover | none | transfer of small companies | mergers obtain permits | **only one kind: "a Foreign Investor merges into a company"**, and it is **triggered automatically** (connected means merged) |
| End condition | bankruptcy or phase | 97% reach Phase IV | bankruptcy/receivership common | **fixed**: after the last 8-train every company operates 3 more times (measured: **100% reach D2/D3, ORs always 10**) |
| Unique mechanisms | — | — | permits | **Building Permits + the Private Company three-layer structure + the Communist Takeover + P7 Rocket of China** |

---

## 3. Turn Order, Auctions and Private Companies (★ Core Questions 1 and 3)

### 3.1 How the provisional order is set, and how much it matters

**Rule (§IV–V)**: at the start of the game, number cards 0–6 are drawn; **the player who draws card 0 takes the big wooden locomotive (the Priority Deal marker)** and is the first actor in the Private Company auction; the other players **seat themselves clockwise** according to the number they drew.

**The rotation of who opens each auction**: P0 is opened by the Priority Deal holder, P1 is opened by **the opener's left-hand neighbour**, P2 by that neighbour again…, i.e. `opener of the i-th Private Company = provisional order[i mod number of players]`.

**Empirical check**: across 2685 auctions in the 341 games, **98.4% (2642/2685)** of openers match the rotation formula above (the 43 exceptions are "unsold → re-auction" auctions). **Every turn-order basis in this report is taken from the `Priority order:` line in the log**, not inferred.

**What does the provisional order actually affect?** — **it only decides "who speaks first", and does not change the auction result**:

| Observation | Data |
|:--|:--|
| The outcome of the first-in-order (card 0) player | top-two rate **45.5%** (155/341), mean rank 2.78 — **no difference from the all-players baseline of 46.0%** |
| The first-in-order player's spending on Privates | median ¥145, mean ¥162 — **no significant difference** from all players (1.99 Privates per head, mean spending) |
| The companies the first-in-order player founded | the top 8 combinations are all Beijing-family and Harbin-family ones such as "JHU／JHA+BZU／JHU+NJR／BZU", **no systematic preference** |

> **Conclusion**: **being "first in order" is neither an advantage nor a disadvantage in this title** — because the auction **requires a bid** (see 3.3: the minimum bid is never lowered), and opening first means showing your cards first; and the total auction spend on the 8 Private Companies is shared by the whole table, so the order does not change "who can afford it". **What really decides your fate is "how much you spent on Privates" and "which companies you got"** — see §3.5 and §5.

### 3.2 The full bid-premium table (**winning bid − minimum bid**) (**Question 3**)

> **Basis** (as specified by the user): **bid premium = final price obtained in the auction − minimum bid**. Minimum bid = the face value printed on the card (P0 ¥5, P1 ¥10, P2 ¥25, P3 ¥45, P4 ¥70, P5 ¥100, P6 ¥160, P7 ¥50).
> **In the 341 games measured, "minimum bid decreases" (`minimum bid decreases`) occurred 0 times** (there is a dedicated check in §3.3), so **this title's minimum bid always equals face value, and bid premium = winning bid − face value**.
>
> ⚠️ **A related difference between the rulebook and the platform implementation**: rulebook §V requires that "**all bids must be multiples of 5**", but the 18xx.games implementation (`selection_auction.rb`: `min_bid = previous bid + MIN_BID_INCREMENT(5)`) **only enforces "each raise ≥5", not that the bid itself is a multiple of 5**. Measured across 2728 auctions, **4 had winning bids that were not multiples of 5** (`1880-084`'s P4 won at ¥104, P5 at ¥136, P7 at ¥169; `1880-298`'s P7 won at ¥151) — **played face to face by the rulebook, these 4 bids are illegal**.

| Private Company | Minimum bid (face value) | Sample | **Times bid premium = 0** | **Share = 0** | Min | Median | Mean | Max | **Median premium rate** |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| **P0** Woosong Railway | 5 | 341 | **0** | **0.0%** | +5 | **+25** | +24.3 | +60 | **5.00×** |
| **P1** Kaiping Railway | 10 | 341 | **10** | **2.9%** | **0** | +10 | +10.0 | +40 | 1.00× |
| **P2** Yanda Ferry Company | 25 | 341 | 1 | 0.3% | **0** | +20 | +22.5 | +65 | 0.80× |
| **P3** Taiwan Western Line | 45 | 341 | 5 | 1.5% | **0** | **+30** | +28.1 | +65 | 0.67× |
| **P4** Chinese Rivers Ferry Companies | 70 | 341 | 3 | 0.9% | **0** | **+30** | +29.6 | +95 | 0.43× |
| **P5** Jeme Tien Yow Engineer Office | 100 | 341 | 6 | 1.8% | **0** | +25 | +28.8 | **+145** | 0.25× |
| **P6** Imperial Qing Government | 160 | 341 | 7 | **2.1%** | **0** | **+45** | +43.4 | +85 | 0.28× |
| **P7** Rocket of China | 50 | 341 | 1 | 0.3% | **0** | **+45** | +44.5 | **+119** | **0.90×** |
| **Total** | — | **2728** | **33** | **1.2%** | **0** | **+25** | **+28.9** | **+145** | — |

### 3.3 The distribution of bid premiums: **98.8% positive, 1.2% zero, no negatives** (**Question 1**)

**Bid-premium distribution across all 2728 auctions**:

| Bid premium | Auctions | Share | Histogram |
|:--|--:|--:|:--|
| **0 (won at the minimum bid)** | **33** | **1.2%** | █████ |
| +5 | 113 | 4.1% | ███████████████████ |
| +10 | 274 | 10.0% | ███████████████████████████████████████████████ |
| +15 | 219 | 8.0% | ██████████████████████████████████████ |
| +20 | 367 | 13.5% | ████████████████████████████████████████████████████████████████ |
| **+25** | **400** | **14.7%** | ██████████████████████████████████████████████████████████████████████ |
| +30 | 368 | 13.5% | ████████████████████████████████████████████████████████████████ |
| +35 | 251 | 9.2% | ███████████████████████████████████████████ |
| +40 | 204 | 7.5% | ███████████████████████████████████ |
| +45 | 164 | 6.0% | ████████████████████████████ |
| +50 ~ +55 | 211 | 7.7% | ████████████████████████████████████ |
| +60 ~ +80 | 104 | 3.8% | ██████████████████ |
| **> +80 (long tail, max +145)** | 18 | 0.7% | ███ |
| (+34 and +36, 1 auction each; see the note on non-multiples of 5 above) | 2 | 0.1% | — |

**Quantiles**: p10 = **+10**|p25 = **+20**|**median = +25**|p75 = **+40**|p90 = **+50**|max **+145**.

**Four conclusions from reading the table**:

1. **Auctions with a bid premium of 0 do exist, but only 33 (1.2%)** — **and they are all concentrated in the "only 1 bidder" auctions** (see the table below). **As soon as 2 or more bidders take part, the bid premium is never 0.**
2. **P0 is the only company with "zero minimum-bid wins"** (0/341), and it has a **median premium rate of 5.00×** (face value ¥5 → median winning bid ¥30) — **every single P0 was fought over.**
3. **The median bid premium is only +¥25 and the mean +¥28.9**, meaning **the great majority of auctions are mild contests settled at "face value +25 or so"**; **only 12.2% (333/2728) reach +¥50 or above**, and within that the >¥80 long tail is only 18 auctions (0.7%).
4. **The harshest absolute premiums are P6/P7 (median +¥45 each)**, but **the harshest relative premium rates are P0 (5×) and P7 (0.9×)** — P7 Rocket of China is the model of "cheap, in demand, high premium" (face value ¥50, median winning bid ¥95).

**Number of bidders → bid premium** (key cross-tabulation):

| Number of bidders | Auctions | Median bid premium | Mean bid premium | **Share of these = 0** |
|:--|--:|--:|--:|--:|
| **1 (no competition)** | 160 | **+15** | +16.6 | **20.6% (all 33 are here)** |
| 2 | 803 | +20 | +23.8 | **0.0%** |
| 3 | 1051 | +25 | +29.5 | **0.0%** |
| 4 | 576 | +32 | +36.0 | **0.0%** |
| 5 or more | 137 | **+40** | +39.3 | **0.0%** |

> **"Won at the minimum bid" = "only one person at the whole table wanted it"**. Of the 33 minimum-bid wins: **60.6% (20/33) happened in 3-player games** (fewer people → less competition), spanning 21 games, and in those games the **median total Private Company winning bids is ¥545, far below the ¥700 for all games** — **they are a product of "quiet games", not a "bargain-hunting skill"**.

**Bid premium → buyer outcome**:

| Bid premium | Buyer-games | Top-two rate | Mean rank |
|:--|--:|--:|--:|
| **0 (minimum bid)** | 33 | 57.6% | 2.30 |
| +5 ~ +20 | 973 | 53.3% | 2.51 |
| +25 ~ +45 | 1389 | 47.3% | 2.74 |
| **≥ +50** | 333 | **38.7%** | 2.87 |

> **The higher the premium, the worse the buyer's outcome** (57.6% → 38.7%, monotone) — **this too must not be read as causation**: the high-premium auctions are mostly the "must-haves" P6/P7, and **the buyer does not lose "because he paid more", but "because he emptied his cash to grab it"** (the two sides of the same thing as §3.5's "spending down to zero cash is the only red line").
>
> **Two facts that must coexist**: ① the minimum-bid buyers' top-two rate is 57.6%, which **looks very good** — but 60.6% of these 33 auctions are in 3-player games, and **the top-two-rate baseline in a 3-player game is itself 66.7%**; computed on the player-count composition of these 33 auctions, **the expected baseline is 57.9%** — in other words, **the minimum-bid buyers' outcome matches the baseline exactly (57.6% vs 57.9%): they neither gained nor lost**. ② The sample is only 33 auctions / 21 games, **for reference only**.

### 3.4 The auction landscape: **"3 bidders fighting" is the norm, "only 1 bidder" is the anomaly**

- **The median number of bidders per auction is always 3** (it holds for 3- to 7-player games alike): 1051/2728 = **38.5%** of auctions have 3 bidders, 576 (21.1%) have 4, and 137 (5.0%) have 5 or more; **"cold auctions" with only 1 bidder number 160 (5.9%)**.
- **The number of bids rises with the face value** (P0 3 bids → P7 7 bids): **cheap companies are "a quick snatch, over at once", expensive ones are "a grinding exchange of probes"**.
- **In 54%–76% of auctions the opening bid is simply the face value** — players open at the minimum bid by default, and do not set a "tentative low bid".
- **1.99 Privates per head**: 1 Private: 461 player-games|2: 559|3: 260|4: 77|5: 11|**6: 1** (the only instance: `1880-165` nestorjj → 1st). **All 8 Privates sold, 100%** — "buying none at all" is not an option.
- **Holding several Privates does not hurt you** (see the number-held table in 3.5), but it is **not necessary** either: 2 per head is the table-wide median, and the counts of auctions with 1 and with 3 are comparable.

### 3.5 How should you buy Private Companies: **by "number of Privates held" it improves monotonically; only "spending down to zero cash" is harmful** (**the other half of Question 1**)

> ⚠️ **Note on basis**: analysing "how much was bought" must be done **by number held (the combination basis)**, not by amount — because in this title starting capital in 3- to 7-player games ranges from ¥600 to ¥300, so the same ¥400 is 67% of one's funds in a 3-player game and 133% in a 7-player game. **Both tables below are normalized by player count.**

**① By "number of Private Companies bought"**

| Number of Privates held | Player-games | **Top-two rate** | Mean rank | Median spend |
|--:|--:|--:|--:|--:|
| **0 (none at all)** | 115 | **41.7%** | 3.03 | ¥0 |
| 1 | 461 | 41.4% | 2.98 | ¥95 |
| 2 | 559 | 46.5% | 2.75 | ¥160 |
| 3 | 260 | **50.0%** | 2.55 | ¥220 |
| 4 | 77 | **55.8%** | 2.40 | ¥320 |
| 5 | 11 | **81.8%** (small n) | 1.82 | ¥375 |
| 6 | 1 | 100% (n=1) | 1.00 | ¥385 |

**② By "Private spending ÷ starting capital"**

| Spend/starting capital | Player-games | Top-two rate | Mean rank |
|:--|--:|--:|--:|
| 0% | 115 | 41.7% | 3.03 |
| ≤30% | 536 | **48.1%** | 2.71 |
| 30–50% | 473 | 43.8% | 2.84 |
| 50–83% | 322 | **48.4%** | 2.69 |
| **>83% (almost emptied)** | 38 | **34.2%** | 3.29 |

**Three things to read out of this**:

1. **"The more you buy, the better" is monotone** (0 held 41.7% → 4 held 55.8%, a span of 14 percentage points) — **buying Privates is not a burden but positioning**: Private Companies provide **private income** (credited every OR, valid for life until Phase B3) and **compounding abilities** (P2 free ferry, P3 Taiwan +20, P4 river track-laying −¥20), and on top of that **every cent spent turns into the right to "pick your investor first"** (§4.1: the one with the least money picks first).
2. **The only red line is "spending down to zero cash" (>83%, top-two rate 34.2%, n=38)** — players in this band founded a median of **0** companies in SR1 and made a median of **0** share purchases, **which amounts to handing the whole of the first Stock Round to the others**. **The boundary for buying Privates is "after buying, there is still enough left to float one company".**
3. **Buying nothing at all (41.7%) is just as bad as buying only 1 (41.4%)** — these two bands account for 38.8% of all player-games (576/1484), and are **the two most common "mediocre choices"**.

> **Cases (5 or more held, with a good outcome)**: `1880-063` (Player30 bought 5 → 1st), `1880-083` (cougar bought 5 → 1st), `1880-086` (Player14 bought 5 → 1st), `1880-165` (nestorjj bought 6 → 1st), `1880-336` (Player16 bought 5 → 1st).
> **Counter-cases (spending down to zero cash)**: see the P1+P5 combination in §3.6 (top-two rate 34.5%) and the counter-case list in §8.3.

### 3.6 Private Company combinations (**combination statistics, not frequencies**)

| Combination (bought by the same player) | Instances | Top-two rate | | Single | Instances | Top-two rate |
|:--|--:|--:|:--|:--|--:|--:|
| P0+P6 | 31 | **58.1%** | | **P3 Taiwan Western Line** | 341 | **59.5%** |
| P4+P6 | 27 | 55.6% | | P0 Woosong Railway | 341 | 51.6% |
| P2+P3 | 26 | **69.2%** (small n) | | P6 Imperial Qing Government | 341 | 51.0% |
| P3+P7 | 28 | 50.0% | | P2 Yanda Ferry Company | 341 | 47.5% |
| P1+P5 | 29 | 34.5% | | **P5 Jeme Tien Yow Engineer Office** | 341 | **41.1%** |
| **P5 alone** | 89 | **41.6%** | | P1 Kaiping Railway | 341 | 44.0% |
| **P2 alone** | 47 | **27.7%** | | P4 Chinese Rivers Ferry Companies | 341 | 46.3% |

**Key points**:

- **At the single-Private level: P3 (Taiwan Western Line, ¥45 face value, +20 Taiwan value) has a buyers' top-two rate of 59.5%, the highest single Private in the whole title**; **P5 (Jeme Tien Yow Engineer Office, ¥100, a Phase D Building Permit) is the lowest (41.1%)** — a permit that only takes effect in Phase D **comes too late**.
- **P6's value is not in itself**: P6 as a single Private has a top-two rate of 51.0%, but **P0+P6 (58.1%) and P4+P6 (55.6%)** are clearly better — because **P6's real payoff is "getting the BCR Director's Certificate for free"** (see §4.4), and it requires **simultaneously** holding P0 (a one-off payment: P0's ¥40/70/100 exactly covers P6's low-income gap) or P4 (the river track-laying discount, used on BCR's Chengkun line) before it can be cashed in.
- **P2 alone is the worst (27.7%)**: the free ferry has a narrow range of application (ferry connections on the map are limited), and P2's ¥25 face value / ¥10 income is poor value for money.

### 3.7 Private Company by Private Company: **different bid premium → different outcome** (**Question 4**)

Banding each company's bid premium (low = below the median, mid = median to p75, high = above p75) and looking at the **buyers'** top-two rate:

| Private Company | **Low band** | Mid band | **High band** | Trend |
|:--|--:|--:|--:|:--|
| **P0 Woosong Railway** | **60.9%** (n=233, +5~25) | 40.4% (52, 26~30) | **23.2%** (56, >30) | **the higher the premium, the worse; span 38pp** |
| P1 Kaiping Railway | 43.4% (265, +5~10) | 40.0% (10, =0) | 47.0% (66, >10) | **essentially unrelated** (P1 is cheap, the premium does not matter) |
| **P2 Yanda Ferry Company** | **56.8%** (190, +5~20) | 37.5% (88, 21~25) | 33.9% (62, >25) | **monotone decline** |
| **P3 Taiwan Western Line** | **60.3%** (232, +5~30) | **61.0%** (59, 31~35) | 55.6% (45, >35) | **high throughout** — just buy it, the price does not matter |
| P4 Chinese Rivers Ferry Companies | 52.2% (232, +5~30) | 28.6% (42, 31~35) | 34.4% (64, >35) | a low price is clearly better |
| **P5 Jeme Tien Yow Engineer Office** | 36.8% (171, +5~25) | 44.0% (84, 26~35) | 45.0% (80, >35) | **the only one that is "better the dearer it gets"** (but all of it is below the baseline) |
| **P6 Imperial Qing Government** | **56.5%** (214, +5~45) | 49.1% (55, 46~50) | **32.3%** (65, >50) | **the higher the premium, the worse; span 24pp** |
| **P7 Rocket of China** | **53.3%** (210, +5~45) | 45.6% (68, 46~55) | **27.4%** (62, >55) | **the higher the premium, the worse; span 26pp** |

**Three actionable conclusions**:

1. **Except for P1/P5, every Private is "bought dear = worse outcome"**, with **P0 (+5~25 → 60.9%, >30 → 23.2%), P6 and P7 having the largest spans (24–38pp)**.
2. **P3 (Taiwan Western Line) is the only Private in the whole title that "is worth buying at any price"**: the low/mid/high bands are 60.3% / 61.0% / 55.6% respectively, all clearly above the 46.0% baseline.
3. **P5 (Jeme Tien Yow Engineer Office) is better the dearer it gets, yet below the baseline throughout** (36.8%→45.0%, still <46%) — **its value has to wait for Phase D, and this title's era D is only 3 rounds long**, so however you buy it, it does not pay off.

**TOP players' bids vs everyone else** (**the "strong-player case" of Question 4**):

| Private Company | TOP median bid premium | Other players' median | Difference |
|:--|--:|--:|:--|
| P0 Woosong Railway | +25 | +25 | 0 |
| P1 Kaiping Railway | +10 | +10 | 0 |
| P2 Yanda Ferry Company | +20 | +20 | 0 |
| P3 Taiwan Western Line | **+30** | +25 | **+5** |
| P4 Chinese Rivers Ferry Companies | +30 | +30 | 0 |
| P5 Jeme Tien Yow Engineer Office | **+20** | +25 | **−5** |
| P6 Imperial Qing Government | **+48** | +45 | **+3** |
| P7 Rocket of China | **+40** | +45 | **−5** |

> **TOP players' bids are almost identical to everyone else's** — only slightly higher on P3/P6 (+3~+5) and slightly lower on P5/P7 (−5). **In other words: strong players do not win by "bidding harder", but by "stopping at the right price"**: on P6 they are willing to pay a little more (because it locks in BCR), while on P7/P5 they bid more conservatively (because the value is realised too late).

### 3.8 Opening bids: **"opening at face value" is the norm, but once an opening is made the price will certainly be bid up** (**Question 1**)

| Private Company | Face value | n | **opening = face value** | **opening > face value** | median opening | **highest opening bid** | **winning bid / opening** |
|:--|--:|--:|--:|--:|--:|--:|--:|
| **P0 Woosong Railway** | 5 | 341 | 54.3% | **45.7%** | 5 | **40 (8× face value)** | **3.00** |
| P1 Kaiping Railway | 10 | 340 | 64.7% | 35.3% | 10 | 30 | 1.50 |
| P2 Yanda Ferry Company | 25 | 341 | 56.3% | 43.7% | 25 | **70** | 1.60 |
| P3 Taiwan Western Line | 45 | 341 | 61.9% | 38.1% | 45 | **95** | 1.44 |
| P4 Chinese Rivers Ferry Companies | 70 | 341 | 66.3% | 33.7% | 70 | 110 | 1.29 |
| **P5 Jeme Tien Yow Engineer Office** | 100 | 341 | **76.2%** | **23.8%** | 100 | 150 | **1.25** |
| P6 Imperial Qing Government | 160 | 341 | 66.9% | 33.1% | 160 | **215** | 1.25 |
| **P7 Rocket of China** | 50 | 341 | 66.6% | 33.4% | 50 | 115 | **1.70** |

**Four conclusions**:

1. **The median opening bid is = face value for every company**, and **54%–76% open at face value straight away** — players by default start calling from the floor price, and **do not set a "tentative low opening"** (the auction rules themselves also forbid bidding below face value).
2. **P0 is the only company whose opening is bid up outright in nearly half of all games** (45.7% opening > face value, highest opening **¥40 = 8× face value**) — **a cheap hot item: the moment a player opens, they raise the price outright**, leaving no chance for anyone else. P5 (Jeme Tien Yow) is the opposite, **76.2% opening at face value** — nobody is in a hurry to grab it.
3. **The multiple of "winning bid ÷ opening bid" reveals the intensity of the contest**: **P0 3.00×** (opens at ¥5 → median winning bid ¥30), P7 1.70, P2 1.60, P1 1.50; **P5/P6 only 1.25×** (large lots get gentle bid-ups).
4. **"Opening at face value" is almost equivalent to "will certainly be bid up"**: among the games where the opening = face value, the share that in the end **still closes at face value is only 0%–4.5%** (P0 **0.0%**, P7 0.4%, P2 0.5%). → **As long as someone opens, this card will inevitably be bid up**.

**A high opening bid → fewer subsequent raises (but a higher final price)**:

| Opening bid | n | median further raise afterwards | **median final bid premium** |
|:--|--:|--:|--:|
| = face value | 1749 | +¥25 | **+¥25** |
| = face value +5~20 | 772 | +¥10 | +¥25 |
| **> face value +20** | 206 | +¥8 | **+¥40** |

> **Opening high = suppressing competition**: calling the opening straight to above face value +20 leaves a median of only +¥8 in subsequent raises (against +¥25 for opening at face value) — **but the final winning bid is still higher (+¥40 vs +¥25)**. **"The jump bid scares opponents off" does have an effect in the data (fewer raises), but it is not enough to offset the cost of the high opening itself**.

**The case of the opening bidder**:

- The opening bidder is determined by the Priority Deal rotation (§3.1), and **the skill composition of opening bidders is the same as the whole population** (top 13%, mid 16%, bot 14%, small 58%, identical to the overall player distribution) — **opening is not "a top player's opportunity"**.
- **The share of openings that the opening bidder himself ends up winning is only 19%–28%** (P7 lowest at 19.1%, P3 highest at 27.9%) — **the opening bid is more often "carrying the sedan chair for someone else" than a "priority purchase right"**.

### 3.9 Pairings of Private Companies: **which companies the buyers later founded** (**Question 2**)

Cross-tabulating "the player who bought a given Private Company" with "the companies of which he is Director", and using **lift** (actual share ÷ whole-population baseline share) to measure pairing strength:

| Private Company | companies the buyer most often founds (share ×lift) | Reading |
|:--|:--|:--|
| **P6 Imperial Qing Government** | **BCR 33% (×3.91)**, CKR 15% (×1.97) | **strongest pairing** — P6 gives away the BCR Director's Certificate for free, and the buyer casually founds CKR along the Chengkun line |
| **P3 Taiwan Western Line** | **HKR 18% (×2.10)**, SCR 13% (×1.80), NJR 12% (×1.45) | Taiwan's +20 value has to be cashed in via the **south-east coastal route** → Shanghai/Changsha/Nanjing |
| **P2 Yanda Ferry Company** | HKR 12% (×1.50), JHU 10% (×1.21), **QSR 10% (×1.39)** | free ferry → coast + north-east (the ferry is in the Bohai Bay) |
| **P7 Rocket of China** | **JHU 13% (×1.56)**, JHA 12% (×1.46), JLR 9% (×1.13) | inter-company train transfer ability → **the Beijing group** (four companies in Beijing fight over track; a train transfer can tide you over) |
| **P5 Jeme Tien Yow Engineer Office** | JHA 11% (×1.29), JHU 10% (×1.21), **BZU 9% (×1.33)** | Phase D permit → the Beijing/Harbin group (the north has the greatest track-laying demand) |
| **P4 Chinese Rivers Ferry Companies** | NJR 11% (×1.31), CKR 9% (×1.17), WNR 9% (×1.25) | river discount → along the Yangtze (Nanjing/Wuhan) and the south-west (Chengdu/Kunming) |
| **P1 Kaiping Railway** | HKR 11% (×1.28), JHU 9% (×1.14), SCR 8% (×1.12) | no ability, pure cash flow → slightly towards the south-east |
| **P0 Woosong Railway** | NJR 10% (×1.21), JLR 10% (×1.22), BCR 10% (×1.18) | **weakest pairing** (all lifts around 1.2) — a one-off payment ability gives no indication of "which company to found" |

**Private Company → Foreign Investor (with lift, **Question 5**)**:

| Private Company | the investor the buyer most often picks (share ×lift) | Mechanism |
|:--|:--|:--|
| **P6** | **French 71% (×3.44)** | the French Investor's Home Base is French Indochina → the fastest connection to BCR (Chengdu) |
| **P3** | **British 49% (×2.89)** | the British Investor's Home Base is Hong Kong → the fastest connection to HKR (Shanghai) |
| **P2** | **Japanese 17% (×2.34)**, German 25% (×1.90) | the Japanese Investor's Dalian (Bohai Bay) / the German Investor's Qingdao run in the same direction as P2's ferry corridor |
| **P5** | Russian 19% (×1.56) | the Russian Investor's Russia (A3) → the north, matching the fact that P5 buyers mostly found Beijing/Harbin-group companies |
| **P4** | Belgian 17% (×1.49), French 25% (×1.19) | the Belgian Investor's Taiyuan / the French Investor's south-west, corresponding to the Yangtze–south-west corridor |
| **P7** | German 20% (×1.54), Russian 18% (×1.44) | the buyers sit late in the turn order and can only pick German/Russian |
| **P0 / P1** | no significant preference (lift ≈ 1.0–1.28) | the buyers of these two are spread out across the turn order |

> **Taken together, the two tables say**: **a Private Company's ability → determines which corridor the buyer expands along → determines which investor he pairs with**. This is not "players choosing at random" but **a chain locked in place by the map's topology** (entirely consistent with the station placement of §7.7).

---

## 4. Foreign Investors: the Draft's Logic and Results (★ Questions 1, 2)

### 4.1 Draft order = priority order (341/341, **zero exceptions**)

Rules §V: after the Private Company auction ends there is a **re-seating** — **the player with the least money takes the Priority Marker** and becomes first, the rest are seated in ascending order of money; then **in this order they choose Foreign Investors for free, one after another**.

**Measured**: in the 341 games, the order given by the `Priority order:` line and the order in which players actually chose investors in the log are **exactly the same (341/341)**. Therefore:

| Observation | Data |
|:--|:--|
| **The 1st-priority player's Private Company spending** | **100% falls in the ">¥150" bucket** (341/341) — this is not a coincidence, it is the rule: **spend more → have less money → choose earlier** |
| The 1st-priority player's investor choice | **French 220/341 = 64.5%**, Portuguese 48, British 23, Belgian 17, German 15, Russian 13, Japanese 5 |
| **The player who chooses last** | necessarily the one who spent the least on Privates (usually the one who "passed on everything"), and can only take what is left |

> **This is this title's most easily overlooked compensation mechanism**: **the money spent on Private Companies returns part of its value in the form of "choosing an investor first"**. The buyer of P6 (spending ¥160–245) is almost always the first to choose, so he almost always gets the **French Investor** (see 4.3).

### 4.2 Each investor → the chooser's outcome

| Investor | Home Base | Times chosen | Chooser's top-two rate | Chooser's mean rank |
|:--|:--|--:|--:|--:|
| **Portuguese** | Macau (P12) | 273 | **54.6%** | **2.44** |
| **British** | Hong Kong (Q15) | 253 | **52.6%** | **2.57** |
| French | French Indochina (Q7) | 307 | 48.5% | 2.64 |
| Russian | Russia (A3) | 184 | 44.6% | 2.86 |
| German | Qingdao (H14) | 195 | 39.0% | 2.99 |
| Belgian | Taiyuan (H6) | 166 | 36.1% | 3.22 |
| **Japanese** | Dalian (E13) | 106 | **31.1%** | **3.39** |

**Why are Portugal／Britain the best?** Because their **Home Bases are on China's south-east coast (Macau, Hong Kong)**, and this stretch is **the necessary path of the Hukun line (HKR, Shanghai) and the Jingguang／Nanjiang (NJR, Nanjing)** — the south-east coast has the best connectivity, and the investor **can form a joint network with a company and complete the merge fastest** (merge = treasury transfer + ¥50 bonus + a 10% share). Japan (Dalian) and Belgium (Taiyuan) are further north/inland, so they connect slowly.

### 4.3 Investor × associated company matrix (where the merge goes)

| Investor | Company merged with most often | Times | Note |
|:--|:--|--:|:--|
| **French** | **BCR (Chengdu)** | **243** | the French Home Base Q7 (French Indochina) → connects to Chengdu in the south-west |
| British | HKR (Shanghai) | 114 | Hong Kong ↔ the Shanghai coast |
| Russian | JHA (Beijing) | 96 | Russia A3 ↔ Beijing |
| Portuguese | HKR (Shanghai) | 84 | Macau ↔ Shanghai |
| German | JHU (Beijing) | 67 | Qingdao ↔ Beijing |
| German | HKR (Shanghai) | 61 | |
| Portuguese | BCR (Chengdu) | 57 | |
| Belgian | JGG (Beijing) | 53 | Taiyuan ↔ Beijing |

> **This table is a practical quick reference for "investor—company pairings"**: **the French investor almost locks onto BCR** (243/307 = 79%), **the British/Portuguese investors prefer HKR** (Shanghai), **the Russian/German/Belgian investors prefer the four Beijing companies (JHA/JHU/JGG/JLR)**.

### 4.4 **P6 → BCR is a locked pairing with zero exceptions**

Rules: the owner of P6 **directly receives BCR's 20% Director's Certificate**, and BCR's starting price is automatically set to **100**.

| Check item | Data |
|:--|:--|
| The P6 buyer chooses the **French Investor** | **71%** (the P6 buyer is almost always first in the order, and takes the French investor) |
| **Games in which someone else (not the P6 holder) bought the BCR Director's Certificate** | **0 games / 341** |
| The final outcome of the player holding P6 | top-two rate **51.0%** (174/341), slightly above the 46.0% baseline |

> **Conclusion**: **"buy P6 = you will become BCR's Director" is this title's most certain causal chain** — in not one of the 341 games did "the P6 holder give the BCR Director's Certificate to someone else" occur. There are two reasons: ① the rules allow the holder to take the certificate straight over during his own share-buying turn (at no cost); ② BCR (Chengdu, red) is the strongest company in the title (median final share price **200**, median total dividends ¥4410, see §5).
>
> **Conclusion for players**: **a reasonable bid for P6 should include the option value of "getting BCR"**. Measured, P6's median winning bid is ¥205, while one 20% Director's Certificate of BCR bought directly in SR1 costs ¥200 — that is, **P6 is equivalent to "buying BCR's right to start + an exclusive monopoly with your own cash"**. This is the clearest bargain in the whole game.
>
> **Cases** (the P6 buyer is the BCR Director; no exception in 341 games): `1880-001` (nestorjj takes P6 for ¥210 → becomes BCR Director himself → 2nd), `1880-002` (Player24 takes P6 → BCR Director → 2nd), `1880-003` (Player29 takes P6 → BCR Director → 2nd).

### 4.5 The economics of the merge

| Item | Data |
|:--|:--|
| How the merge is triggered | **Automatic** (triggered on connection; the player can only choose where the treasury goes) |
| What the owner always gets | **a ¥50 connection bonus + the 10% share reserved for the investor** (whoever is Director, see rules §VII) |
| **Where the treasury goes, measured** | **transferred in full to the company 1413 times (95.2%) vs the owner taking 20% only 71 times (4.8%)** |
| Amount transferred to the company | median **¥380** (mean ¥356, max ¥870) |
| Amount taken as 20% | median **¥40** (mean ¥49, max ¥124) — **all of them early merges with a small treasury** |
| Merge timing | median **OR4** (the batch that took 20% is on the early side, median OR3) |
| **Investors' final merge rate** | **99.4%–100% (by investor)** — across all 341 games only 3 investors failed to merge |

> **Two conclusions**:
> ① **the "20% option" is almost never used in practice** (4.8%): the investor's treasury is itself only the operating income of a few withholds, and 20% of it is often only ¥40 — far worse than leaving it to the company to buy its next train.
> ② **the rule "not merged by the end of the game" essentially never triggers** (merge rate 99.4–100%) — **investors will certainly merge in**, so when choosing an investor you should calculate on the basis of "which company it will merge into", not on the basis of "what if it doesn't merge".

### 4.6 Sequence, turn order and the pairing chain (**Questions 1, 5**)

**First, answering the sequence question: the investors are chosen "after all 8 Private Companies have been auctioned", not one chosen per auctioned Private.**

| Check | Result |
|:--|:--|
| Rules (§V) | "Once all the Private Companies have been auctioned, a new seating order … The player with the Priority Marker is the first to choose a Foreign Investor" |
| Log evidence | the `Priority order: …` line appears **after the last auction is won** and **before the first draft**; in 341 games, **the number of games where draft order ≠ priority order = 0** |

> Therefore the second scenario the user imagined ("the bid contains a valuation of the right to choose an investor first") **cannot directly occur in the sequence** — when the Privates are auctioned, the investors have not yet been chosen. **But there is a mechanically equivalent chain**: the order of choosing is determined by "**who has the least money after the Private auction**", and **least money = spent the most on Privates**. So **every penny of extra spending on Privates buys not only the Private Company itself, but also "an earlier draft position"** — that is the real mechanical vehicle of that valuation.

**Measured data for the chain (three segments)**:

| Draft position | Median Private spending of the player at that position | Investor most often obtained | Top-two rate |
|:--|--:|:--|--:|
| **1st position** | **¥295** | **French 65%**, Portuguese 14%, British 7% | **51.0%** |
| 2nd position | ¥190 | British 24%, Portuguese 23%, German 16% | 45.2% |
| 3rd position | ¥130 | British 22%, Russian 18%, Portuguese 18% | 46.3% |
| 4th position | ¥82 | Portuguese 22%, British 20%, Belgian 15% | 42.5% |
| 5th position | ¥25 | Japanese 22%, Russian 20%, Belgian 16% | — |
| 6th position | ¥18 | Russian 28%, Japanese 25% | — |
| **7th position** | **¥0** | Japanese 29%, German 24% | — |

**Looking at investor availability in three turn-order segments**:

| Turn-order segment | n | Investors available to pick |
|:--|--:|:--|
| **First 1/3** | 644 | **French 40%**, Portuguese 18%, British 14%, German 11% |
| Middle 1/3 | 467 | British 24%, Portuguese 18%, Russian 16%, German 16% |
| **Last 1/3** | 373 | Russian 20%, Portuguese 19%, Belgian 15%, German 14% |

> **The probability of getting the French investor from a "first 1/3 position" is more than 4 times that of the last 1/3** (40% vs none); conversely, **the Japanese/Russian/Belgian investors are basically the choice of players in the later positions**. This explains the "investor—outcome" correlation in §4.2: **the people who got the Portuguese/British/French do better partly because they are "people who spent a lot of money on Privates"** (the other side of spending a lot is being early in the draft) — **the two causal chains are tangled together and you cannot count only one side**.

**Private Company → the Foreign Investor chosen by the buyer (pairing table, **Question 5**)**:

| Private Company | n | Investor the buyer most often chose |
|:--|--:|:--|
| **P0** Woosong Railway | 341 | French 24%, Portuguese 20%, British 17% |
| **P1** Kaiping Railway | 341 | Portuguese 23%, British 20%, French 19% |
| **P2** Yanda Ferry Company | 341 | **German 25%**, British 19%, Japanese 17% |
| **P3** Taiwan Western Line | 341 | **British 49%**, Portuguese 24%, French 13% |
| **P4** Chinese Rivers Ferry Companies | 341 | French 25%, Portuguese 22%, Belgian 17% |
| **P5** Jeme Tien Yow Engineer Office | 341 | Russian 19%, British 17%, Portuguese 17% |
| **P6** Imperial Qing Government | 341 | **French 71%**, Portuguese 17%, British 5% |
| **P7** Rocket of China | 341 | German 20%, Russian 18%, French 16% |

**The mechanical explanation of the pairings (not mysticism)**:

- **P6 → French investor 71%**: the P6 buyer (almost always first in the order) uses P6 to get BCR (Chengdu), and **the French investor's Home Base is French Indochina (Q7), the only investor that can connect to Chengdu fastest** (§4.3: French investor → BCR merge 243 times).
- **P3 → British investor 49%**: the buyers of P3 Taiwan Western Line are almost all people "preparing to found HKR (Shanghai)", and **the British investor's Home Base, Hong Kong (Q15), is the closest to Shanghai**.
- **P2/P7 → German/Russian investor**: the buyers of these two are often not first in the order (P2's median winning bid is only +20, P7 +45), so they cannot get the Portuguese/British/French at the front and can only pick German/Russian.
- **P5 → Russian investor 19%**: P5's buyers are scattered, with no strong preference — **"no preference" is itself a signal: P5's value materialises too late, and players do not pair an investor with it**.

---

## 5. Major Companies: Appearance, Timing, Share Price, Holdings (★ Questions 1, 2)

### 5.1 Appearance rate and founding timing of the 14 companies

| Company | Home Base | Games appeared | Appearance rate | **Median first SR** | **SR1 founding rate** | Median par | par range | Median final share price | Median total dividends |
|:--|:--|--:|--:|--:|--:|--:|:--|--:|--:|
| **BCR** Baocheng | Chengdu (M3) red | 341 | **100.0%** | 0 (comes with P6) | — (see §4.4) | **100** | 100–100 | **200** | **4410** |
| **HKR** Hukun | Shanghai (K15) light green | 338 | **99.1%** | **1** | **89.9%** | **100** | 70–100 | **200** | **4470** |
| JHU Jinghu | Beijing (F8) black | 337 | 98.8% | 1 | 68.9% | 90 | 70–100 | 180 | 3840 |
| JGG Jingguang | Beijing (F8) purple | 336 | 98.5% | 4 | 30.8% | 90 | 70–100 | 115 | 2305 |
| JHA Jingha | Beijing (F8) orange | 334 | 97.9% | 2 | 45.7% | 90 | 70–100 | 125 | 2855 |
| NJR Nanjiang | Nanjing (K13) dark brown | 328 | 96.2% | 3 | 36.1% | 90 | 70–100 | 120 | 2550 |
| JLR Jinglan | Beijing (F8) blue-green | 321 | 94.1% | 6 | 6.2% | 90 | 70–100 | 100 | 1750 |
| CKR Chengkun | Kunming (O5) light blue | 317 | 93.0% | 3 | 20.5% | 90 | 70–100 | 110 | 2170 |
| SCR Schichang | Changsha (N12) dark green | 302 | 88.6% | 4 | 29.6% | 90 | 70–100 | 110 | 2260 |
| QSR Qinshen | Shenyang (D12) yellow | 292 | 85.6% | 5 | 10.3% | 90 | 70–100 | 100 | 1740 |
| **WNR** Wunan | Wuhan (L10) light brown | 287 | 84.2% | 5 | 16.7% | 90 | 70–100 | 105 | 1910 |
| **BZU** Binzhou | Harbin (B8) beige | 266 | 78.0% | 5 | 18.5% | 80 | 70–100 | 105 | 1775 |
| **NXR** Ningxi | Xi'an (J6) pink | 147 | **43.1%** | 7 | 4.8% | 80 | 70–100 | 100 | 1410 |
| **LHR** Longhai | Lanzhou (J2) dark blue | 100 | **29.3%** | 8 | 4.0% | 80 | 70–100 | 85 | 1080 |

**How to read the table**:

1. **Two super-companies**: **BCR (Chengdu) and HKR (Shanghai)** — appearance rates 100%/99.1%, median final share price **200** (the highest in the field; 1.1 times the second band of 180, 2.4 times the lowest band of 85), median total dividends **¥4410/¥4470** (4 times LHR's). **These two are the "game-deciders" of the whole work**.
2. **Two dead-zone companies**: **LHR (Lanzhou, appearance rate 29.3%) and NXR (Xi'an, 43.1%)** — they sit in the north-west / inland, with low route revenue and slow connectivity (LHR's median first SR is **round 8**, almost equal to "nobody founds it").
3. **The "SR1 founding rate" is the true indicator of a company's popularity**: HKR **89.9%** > JHU 68.9% > JHA 45.7% > NJR 36.1% > JGG 30.8% > SCR 29.6% > CKR 20.5% > BZU 18.5% > WNR 16.7% > QSR 10.3% > JLR 6.2% > NXR 2.1% > LHR 1.2%.
4. **The par price is almost invariably 90** (the median par of every company is 80/90); the only exception is **HKR and BCR — median 100**. **par=70 is the "survival price for late-starting companies"** (see 5.3).

### 5.2 "When to found" and "how many companies to found": **the decisive metric**

**11.87 / 14 Major Companies float per game** (3-player 11.31, 4-player 11.69, 5-player 12.19, 7-player 13.00). Aligning a player's **number of companies directed** with the outcome:

| Number of companies directed | Player-games | **Top-two rate** | Mean rank | Median final net worth |
|--:|--:|--:|--:|--:|
| **1** | 114 | **18.4%** | 3.89 | ¥4955 |
| 2 | 520 | 40.0% | 3.00 | ¥5970 |
| **3** | 576 | **49.0%** | 2.65 | ¥6814 |
| **4** | 217 | **61.8%** | 2.22 | ¥8007 |
| **5** | 49 | **65.3%** | 1.98 | ¥8713 |
| 6–7 | 8 | 62.5% | 2.38 | ¥9326 |

> **This is the most monotone, strongest regularity in the whole work: the more you found, the more you win** (1 company 18.4% → 5 companies 65.3%, **a span of 47 percentage points**). It makes mechanical sense: the Director **has sole control of** the "payout vs withhold" decision, **has sole control of** the company treasury (and can buy trains from a company with the same Director), and can build a position in his own company at the par price ahead of others.
>
> **Causal warning**: this one runs **both ways** — a leading player already has the money to found more companies. But **the "found 3 or more" threshold can be actively pursued**: see the TOP play in §8.3 (they are on average Director of **2.90** companies, and they put 39.9% of their starting capital into Private Companies, then turn private income into the capital for founding companies).
>
> **Cases**: `1880-046` (Player16 is Director of **5 companies** → 1st), `1880-058` (Player32 5 companies → 1st), `1880-083` (cougar 5 companies → 1st), `1880-005` (Player7 **4 companies** → 1st), `1880-025` (cougar 4 companies → 1st). Counter-examples: `1880-028` (Player33 directs only 1 company → 4th), `1880-029` (Player31 1 company → 5th), `1880-040` (Player21 1 company → 6th). For the mechanism see **§7.6 inter-company train transfer with the same Director** — multiple companies = a fund pool that can be dispatched between them (**note: the "per-company incidence" of train transfers is unrelated to the number of companies, see §7.6.1 attribution B; the value of multiple companies lies in the "number of channels", not in "unit strength"**).
>
> **But the counter-examples must be honestly flagged**: **directing only 1 company still carries an 18.4% top-two rate** — `1880-032` (Player28 1 company and wins), `1880-043` (nestorjj 1 company and wins), `1880-057` (cougar 1 company and wins) are all the "concentrate your holding in one good company (HKR/BCR)" style rather than the "found companies widely" style. **This path can be walked, but only 1 in 5 people walked it.**

### 5.3 The combination of par price and "Director's Certificate ratio" (**the core association of Question 1**)

When buying a Director's Certificate you fix three things at the same time: **par price (70/80/90/100, each usable 4 times) → certificate ratio (20/30/40%) → Building Permit phases (1–3 consecutive phases)**.

| Director's Certificate | par=70 | par=80 | par=90 | par=100 |
|:--|:--|:--|:--|:--|
| **20%** (permit 3 phases) | 172 times (median founding SR **1**) | 229 (1) | 355 (2) | **928 (1)** |
| **30%** (permit 2 phases) | 136 (2) | 335 (5) | 550 (5) | 318 (2) |
| **40%** (permit 1 phase) | 136 (**7**) | 427 (**7**) | 359 (6) | 101 (6) |

**How to read it**:
1. **par=100 + a 20% certificate = 928 times, the most mainstream opening in the whole work** (median founding SR 1) — **"high face value + small certificate + long permit (3 phases)" is the standard answer**: it has the lowest entry threshold (20% × ¥100 = ¥200) yet guarantees track-laying rights across all three phases A/B/C.
2. **A 40% certificate almost only appears in rounds 6–7** (median founding SR 6–7) — because a 40% certificate costs ¥280–400, it is the "later company plays it safe" choice: take the full 40% in one go, with no need to buy more later.
3. **The 40% certificate at par=70 (¥280)** is the standard kit for "founding a company in the last round".

### 5.4 **Full table of SR1 company founding** (**Question 2**)

**3–4 companies are founded in SR1 each game** (distribution: 2 companies 14 games, **3 companies 130 games**, **4 companies 135 games**, 5 companies 51 games, 6 companies 9 games, 7 companies 2 games). Company by company:

| Company | SR1 founding count | Share of games | **Director's top-two rate** | Director's mean rank | Median final share price | Median total dividends |
|:--|--:|--:|--:|--:|--:|--:|
| **HKR** | **304** | **89.1%** | **59.9%** | **2.32** | 200 | 4640 |
| JHU | 235 | 68.9% | 48.5% | 2.74 | 200 | 4320 |
| NJR | 123 | 36.1% | 43.1% | 2.79 | 190 | 4170 |
| JHA | 156 | 45.7% | 42.9% | 2.85 | 200 | 4230 |
| SCR | 101 | 29.6% | 47.5% | 2.71 | 165 | 3610 |
| **CKR** | 70 | 20.5% | 45.7% | 2.70 | 158 | 3590 |
| BZU | 63 | 18.5% | 38.1% | 3.16 | 160 | 3560 |
| JGG | 105 | 30.8% | **36.2%** | 3.23 | 160 | 3540 |
| WNR | 57 | 16.7% | 33.3% | 3.05 | 160 | 3720 |
| QSR | 35 | 10.3% | 34.3% | 3.26 | 125 | 3020 |
| JLR | 21 | 6.2% | 38.1% | 3.10 | 170 | 3670 |
| NXR | 7 | 2.1% | 28.6% | 3.57 | 120 | 3090 |
| LHR | 4 | 1.2% | 0.0% (n=4) | 4.00 | 118 | 2495 |
| **Baseline** | — | — | **46.0%** | 2.50 | — | — |

**Three actionable conclusions**:

1. **Setting BCR aside, the thing most worth grabbing in SR1 is HKR (Shanghai)**: it is founded in 89.1% of games, **and 59.9% of those who found it reach the top two** (**13.9 percentage points** above the baseline), and its final share price and dividends are both top of the field. **This is the strongest single-point opening conclusion in the whole work**.
2. **JHU/JHA/NJR/SCR/CKR are "pass-mark" choices** (top-two rate 42.9%–48.5%, close to or slightly above the baseline).
3. **JGG is the SR1 trap**: it is founded a great deal (105 times) yet its Director's top-two rate is only **36.2%** (9.8pp below the baseline). **The four Beijing companies (JHA/JHU/JGG/JLR) fight each other for the same tracks**, and JGG (purple) loses out most on the direction of Beijing's exits.
4. **WNR (Wuhan)/QSR (Shenyang)/LHR (Lanzhou)/NXR (Xi'an) are all not worth founding in SR1** (33%–34%); they are **late-starting low-price companies** and should be founded at par=70–80 in rounds 5–8 instead (see 5.6).

### 5.5 After founding: how player holdings evolve (**the "holdings" part of Question 1**)

**The Director's holding at the end of the round in which the company was founded (median)／holding at the end of the game (median)**:

| Company | Median Director's Certificate | **Median Director's holding at the end of that round** | **Median Director's holding at the end of the game** | Difference |
|:--|--:|--:|--:|--:|
| BCR | 20% | 30% | **70%** | +40 |
| HKR | 20% | 40% | **70%** | +30 |
| JHU | 20% | 40% | 70% | +30 |
| JHA | 30% | 40% | 60% | +20 |
| NJR | 30% | 40% | 60% | +20 |
| JGG/JLR/BZU/SCR/CKR/NXR/WNR/QSR | 30–40% | 30–60% | 50% | +0–20 |

**Grouped by par price**:

| par | Sample | Median holding at the end of the founding round | Median holding at the end of the game |
|--:|--:|--:|--:|
| 70 | 444 | 40% | 50% |
| 80 | 991 | 40% | 50% |
| 90 | 1264 | 40% | 50% |
| **100** | **1347** | **30%** | **70%** |

**Grouped by founding timing**:

| Founding SR | Sample | Median holding at the end of the round | Median holding at the end of the game | Director's top-two rate |
|:--|--:|--:|--:|--:|
| SR1 | 1281 | 40% | **70%** | 46.8% |
| SR2 | 208 | 20% | 50% | 49.0% |
| SR3 | 352 | 20% | 60% | 49.4% |
| SR4 | 255 | 30% | 50% | 45.1% |
| SR5 | 514 | 40% | 50% | 50.4% |
| **SR6** | 352 | 50% | 50% | **57.4%** |
| **SR7** | 308 | 50% | 50% | **58.4%** |

**Three key points**:

1. **par=100 companies go "small first, large later"**: at the start they take only 30% (a 20% certificate + one round of adding to the stake is enough to float), but **by the end of the game this rises to 70%** — **high-price companies are "grab the company first, the shares later"**, because par=100 shares are the first to rise to 200, and the cost of adding to the stake rises over time, while the Director has the first-buy right in every SR.
2. **par=70/80/90 companies only reach 50% at the end of the game** — low-par companies have limited upside (mostly 100–120 at the end), **the shareholder structure is dispersed, and the Director does not need to control the majority**.
3. **For companies founded only in SR6/SR7, the Director's top-two rate is 57%–58% (11–12pp above the baseline)** — **"founding a low-price company late" is the second sweet spot of the whole work**: at this point everyone else is throwing money around in era D grabbing trains, while you take 40%–50% of a company at par=70, controlling at low cost a company that can feed off the endgame dividends.

### 5.6 Company strength ≠ the Director's outcome (**the must-read contrast**)

**Break "when it was founded" out and the conclusion flips immediately** (Director's top-two rate, bucketed by founding SR):

| Company | Median final share price | Founded SR1–2 | Founded SR3–4 | **Founded SR5+** | Reading |
|:--|--:|--:|--:|--:|:--|
| **HKR** | **200** | **60%** (184/308) | 47% | 47% | **Strong company: the earlier the better; founded late it means nothing** |
| **JGG** | 115 | **37%** (48/131) | 50% | **61%** | **Weak company: grabbing it in SR1 is a trap** |
| **QSR** | 100 | **34%** (14/41) | 59% | **65%** (132/202) | Weak company: founding it late is the optimal play |
| **JLR** | 100 | 38% | 27% | **60%** | Same as above |
| WNR | 105 | 33% | 43% | 45% | Weak company: founding it late is slightly better |
| BZU | 105 | 38% | 36% | 41% | Weak company, poor in every period |
| CKR | 110 | **53%** | 42% | 42% | Kunming's position is good; **only an early founding has value** |
| BCR | 200 | 50.7% (whole period) | — | — | locked in by P6 (§4.4) |

> **The key distinction**: **"the company itself is strong" and "whether the timing of founding it is right" are two different things, and the latter has the larger effect on the outcome**.
> - **HKR is the only company where "only an early founding pays"**: founded SR1–2 → 60%, founded SR5+ → 47% (a drop of 13pp).
> - **QSR/JLR/JGG are the other way round**: grabbing them in SR1 gives only 34%–38%, **holding off until after round 5 to found them gives a top-two rate of 60%–65%** — 14–19pp above the baseline. The reason is straightforward: when founded late, par need only be 70–80, a 40% certificate takes the full stake in one go, the company does not have to fight others for the same tracks, **and by then you have already saved up enough cash from the dividends of the earlier rounds**.
> - **BZU (Harbin) and LHR (Lanzhou) are the real traps**: whenever they are founded, the top-two rate sits at 36%–41% —**these two should be touched only when they come "handy"** (BZU's appearance rate is 78%, which shows many people were forced to found it).

### 5.7 Change of Director: **0.41 per game, 79.7% "sold out by the incumbent himself"** (**Question 6**)

| Item | Data |
|:--|:--|
| Number of changes | **138 instances / 341 games = 0.405 per game**; **involving 27.3% of games** (93 games); in the remaining 72.7% of games the Director never changed at all |
| Companies where they happened | JGG 17, SCR 17, CKR 16, JHA 13, WNR 12, HKR 11, NJR 11, QSR 10… (**weak companies change hands more readily**) |
| **Timing of occurrence (SR)** | SR1–3 only 8 times \| **SR4 19 \| SR5 24 \| SR6 16** \| SR7 5 \| **SR8 26 \| SR9 19 \| SR10 21** |
| Total | **Concentrated in two windows — the "middle game (SR4–6)" and the "endgame (SR8–10)"**, together 125/138 = 90.6% |

**Classification by cause (on your basis: looking at both parties' actions within the same SR + the incumbent Director's cash)**:

| Type | Count | Share | Median incumbent-Director cash | **New Director's top-two rate** | Example |
|:--|--:|--:|--:|--:|:--|
| **① The incumbent Director sells (sells off / reduces his stake to make way)** | **110** | **79.7%** | ¥415 | 48.2% | `1880-001`/HKR (Fohlenfab→nestorjj), `1880-011`/JGG (Player26→Player27) |
| **② The new Director buys in — takeover by purchase · the incumbent Director has no cash left (cannot afford to follow)** | 14 | 10.1% | **¥58** | **64.3%** | `1880-012`/JGG (Player1→nestorjj), `1880-044`/JHA (Fohlenfab→Player34) |
| **③ The new Director buys in — takeover, incumbent inert · the incumbent Director has money but does not move (incumbent does nothing)** | 14 | 10.1% | ¥285 | **78.6%** | `1880-002`/HKR (Player14→Player24), `1880-025`/CKR (Player16→cougar) |
| ④ No buy or sell on record (other reasons) | 0 | — | — | — | — |

**Two key conclusions**:

1. **"Voluntary sale" is the mainstream (79.7%), and it is not a failure**: the top-two rate of these 110 incumbent Directors is **56.4%** (above the 46.0% baseline) — **in this work, selling a mature company (its share price already up to 150–200) to found a new company is a positive-return trade** (fully consistent with §8.2, "TOP's share-selling frequency is 1.7 times that of ordinary players").
2. **"Being bought out" (②+③, 28 instances in total) is a strongly negative signal**: the top-two rate of these incumbent Directors is only **28.6%** (mean rank 3.43) — the worst-performing group among all their 138 changes of Director. **The difference is not "losing the company" but "the way it was lost"**: selling it yourself = actively rotating your position; being bought out by someone else = you did not keep up with your own company's share price.
3. **"Cannot afford to follow" accounts for only 10.1%** (14 instances, median cash ¥58) — **the main cause of changes of Director is not running out of money but an active reduction of the stake (79.7%)**. Players' cash at the time of the change has a median of ¥366; **most people had the money but chose not to follow**.

> **Practical implication**: if you are the Director of a company and someone keeps adding to their position in your company, **these 28 cases tell you: the price of ignoring it is a top-two rate that falls to 28.6%**. Either follow (keep the Director's post), or actively reduce and rotate your position (that is the path with positive returns).

---

## 6. Cash Flow: Where the Money Comes From, Where It Goes (**Question 4**)

### 6.1 Players' cash curve (**median cash at the start of each SR**)

| Era | Median cash | Median holdings | p10 | p25 | p75 | p90 |
|:--|--:|--:|--:|--:|--:|--:|
| **A** (2 / 2+2 trains) | **¥155** | 30% | ¥50 | ¥90 | ¥255 | ¥350 |
| **B** (3 / 3+3 / 4 trains) | **¥244** | 80% | ¥72 | ¥140 | ¥380 | ¥533 |
| **C** (4+4 / 6 / 6E trains) | **¥369** | 150% | ¥47 | ¥153 | ¥708 | ¥1241 |
| **D** (8 / 8E / 10 trains) | **¥1180** | 190% | ¥310 | ¥675 | ¥1971 | ¥2787 |

**How to read this**:

1. **Era A has the tightest cash** (median ¥155): after you have spent out on Private Companies and finished founding companies, players have almost no spare money in hand — **this is the stage in this title that most demands "getting the numbers exactly right"**; a single wrong par price is enough to leave you unable to afford a share for the entire round.
2. **Era B begins to recover** (¥244); **divergence in era C worsens** (p10 ¥47 vs p90 ¥1241 — **a 26-fold gap**).
3. **Era D cash balloons to ¥1180 (p90 ¥2787)** — because **there is nothing left to buy**: shares are absurdly expensive, the companies are all in era D, and the trains have all been carved up too. **This idle cash generates no return whatsoever** — **the winners are the ones who converted cash into assets before era C ended**.
4. **Total holdings climb all the way from 30% to 190%**: this title **has no Bank Pool**, and shares are traded only with the bank, so **players as a whole keep adding to their positions** (this is also why the gap between the top two in the final standings is only ¥521 — everyone's holdings structure converges in the end).

### 6.2 Company treasuries and withholding rate

| Company | Median final treasury | Median total dividends | **Withholding rate** | Median final share price |
|:--|--:|--:|--:|--:|
| HKR | ¥10 | **4470** | **7.9%** | 200 |
| BCR | ¥10 | **4410** | **9.1%** | 200 |
| JHU | ¥20 | 3840 | 9.3% | 180 |
| NXR | ¥1 | 1410 | 10.9% | 100 |
| LHR | ¥0 | 1080 | 10.9% | 85 |
| BZU | ¥1 | 1775 | 11.3% | 105 |
| CKR | ¥5 | 2170 | **17.8%** | 110 |
| JGG | ¥10 | 2305 | 15.0% | 115 |

**Conclusion**: **"the higher a company's payout rate, the higher its share price"** — the two companies with the lowest withholding rates (HKR 7.9%, BCR 9.1%) happen to be the two with the highest final share prices (both 200); CKR, the one with the highest withholding rate (17.8%), ends at only 110. This is **in the opposite direction** to 1846 ("top withholds more"), because **1880 has no Bank Pool and no bankruptcy**, so shareholders have no need for the company to hold money to prop up the price — **the only source of share-price gains is the payout** (§XIII: payout → the share price moves one space to the right).

### 6.3 Dividend vs withhold: **statistics by era**

| Era | Dividend count | Withhold count | **Withholding rate** | Total dividends | Total withheld | Median revenue per train |
|:--|--:|--:|--:|--:|--:|--:|
| **A** | 4990 | 92 | **1.8%** | ¥454,040 | ¥8,130 | ¥40 |
| **B** | 7520 | 1441 | **16.1%** | ¥1,828,350 | ¥335,010 | ¥90 |
| **C** | 8769 | 3752 | **30.0%** | ¥3,318,510 | ¥1,063,250 | ¥190 |
| **D** | 12546 | 299 | **2.3%** | ¥5,772,320 | ¥94,210 | ¥300 |

> **This is the clearest "era rhythm" in this title**:
> - **Era A: 98.2% payout** — the companies have just started out, and they must use payouts to push the share price up (which also hands cash to the players to go and buy the next company).
> - **The withholding rate in era C soars to 30.0%** — **in order to save up money to buy 6/6E/8 trains** (6-train ¥600, 8-train ¥800); this is the only window in the whole title for "companies hoarding money collectively", and also **the period when players' cash diverges the most**.
> - **Era D immediately returns to a 2.3% payout** — all the trains have been bought, and there is no use in keeping the money, so **full payout pulls the share price up** (era D's total dividends are ¥5.77M, 12.7 times era A's).
> **Practical implication**: if by the end of era C you have not managed to get your company to save up enough money to buy an 8-train, you will be hit by a Forced Train Purchase in era D (see §7.4).

### 6.4 Director contributions: **2.93 per game, 35% crammed into OR10**

| Item | Data |
|:--|:--|
| Total number of contributions (`contributes ¥N`) | **1000 times / 341 games = 2.93 per game** |
| Contribution amounts | total **¥374,199**, median per instance **¥270**, mean ¥374 |
| **Distribution by OR** | OR3–4: 4 times \| OR5: 55 \| OR6: 101 \| OR7: 96 \| **OR8: 190 \| OR9: 200 \| OR10: 354** |
| Personal loans | **only 134 times / 341 games** (0.39 per game), total **¥28,611** |
| Loan interest | 50% (borrow ¥100, owe ¥150) |

> **The OR distribution of contributions is this title's "crisis calendar"**: **it suddenly erupts after OR8** (OR8 190 times, OR9 200 times, **OR10 354 times = 35.4%**). And **there are only 134 loans** — which shows that **the overwhelming majority of Directors contributed out of their own cash** (this explains the surge in players' cash in era D in §6.1: the money was not unspent, it was **spent on the companies' trains**).

### 6.5 Personal loans: **0.39 per game, but the top-two rate of loan-takers is only 19.8%** (**Question 2**)

| Item | Data |
|:--|:--|
| Number of loans | **134 times / 341 games = 0.393 per game** |
| Scope | **86 games (25.2% of games had at least one loan)**, 85 players |
| Amount per loan | median **¥114**, mean ¥214, maximum ¥890; **principal total ¥28,611 → debt at 50% interest ¥42,916** |
| Distribution by OR | OR5 12 \| OR6 27 \| OR7 24 \| OR8 21 \| OR9 22 \| **OR10 28** — **evenly distributed across OR5–OR10** (not concentrated at the end the way contributions are) |
| Per-player borrowing per game | ¥270 (median ¥114); **largest cumulative borrowing in a single game ¥1586** |

**The loan-takers' outcome (the sharpest "danger signal" in this report)**:

| Group | Player-games | **Top-two rate** | Average rank |
|:--|--:|--:|--:|
| **Players who have taken a loan** | 106 | **19.8%** | **3.72** |
| Players who only contributed and never took a loan | 1127 | **48.4%** | 2.71 |
| Overall baseline | 1484 | 46.0% | 2.50 |

> **Loan-takers' top-two rate is only 19.8% — less than half the baseline, and less than half that of "contributors only"**.
>
> **But it must not be read as causation**: a loan is a **symptom of "already having no money"** — in this title a Forced Train Purchase must be made good by the Director, so **being able to contribute from one's own cash (1127 player-games, 48.4%) shows that he still has money; having to use a 50%-interest personal loan (106 player-games) shows that he has already squeezed himself dry in the SRs**. The 1846 report's "contributors at the time had an average rank of 3.29 and a final win rate of 4%" is the same class of phenomenon.
>
> **An actionable criterion**: **once you have to resort to a personal loan, you are already on the road to losing**. The only way to avoid it is the crisis calendar in §7.4 — **have the money for an 8-train ready before OR8** (¥800 per company), rather than waiting until OR10 to borrow at 50% interest.

---

## 7. Behaviour by Era and Crisis Points (**Question 4**)

### 7.1 What are all four eras doing?

> ⚠️ **Basis (user correction, 2026-09-22)**: **later eras have more companies, so absolute counts are systematically inflated** — comparing "era A: 4315 trains vs era C: 6415 trains" directly is misleading. **This table switches to the "company-Operating-Round" as the denominator** (= how many times, on average, each company does something per round), so that what is being compared is **a single company's behaviour in that era**.

**① After normalization (recommended basis): behaviour per company · per Operating Round**

| Era | Company-rounds | Dividends/round | **Withholds/round** | **Withholds as a share of decisions** | **Train purchases/round** | Track lays/round | **Contributions/round** | Median revenue per train |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|
| **A** (2 / 2+2) | 7381 | **0.68** | 0.01 | 1.8% | **0.58** | 0.85 | 0.000 | ¥40 |
| **B** (3 / 3+3 / 4) | 10591 | 0.71 | 0.14 | 16.1% | 0.46 | **1.06** | 0.001 | ¥90 |
| **C** (4+4 / 6 / 6E) | 14533 | 0.60 | **0.26** | **30.0%** | 0.44 | 0.85 | **0.026** | ¥190 |
| **D** (8 / 8E / 10) | 13591 | **0.92** | 0.02 | 2.3% | **0.13** | 0.36 | **0.045** | ¥300 |

**Three new facts that surface after normalization** (invisible in the raw totals):

1. **Era D's "cannot afford to buy trains" is quantified**: the number of train purchases per company per round **falls from 0.58 in A to 0.13 in D** (−78%), while **the contribution rate instead rises to 1.7× that of C (0.045/round)** — **era D companies do not fail to want trains; it is that buying one means the Director pays out of pocket**.
2. **Era C is the only "hoarding era"**: each company chooses to withhold **0.26 times per round** (only 0.01 times in A) — **this is the only window in the whole title in which a company voluntarily gives up paying dividends**.
3. **Era D's dividend rate is in fact the highest (0.92 per round)**: the trains are bought, the money has no use → **dividends every round to push the share price up** (consistent with §6.2, "HKR/BCR have the lowest withholding rate and the highest share prices").

**② Contrast: the raw totals without normalization (inflated by the later eras; for reference only)**

| Era | Train purchases | Track lays | Stations | Player share buys | Player share sells | Foundings | **Withholding rate** |
|:--|--:|--:|--:|--:|--:|--:|--:|
| A | 4315 trains | 6277 | 3182 | 5170 | 282 | 2182 | 1.8% |
| B | 4872 | 11205 | 3387 | 10000 | 1157 | 1121 | 16.1% |
| C | 6415 | 12364 | 2488 | 9787 | 2263 | 713 | 30.0% |
| D | 1731 | 4938 | 213 | 1545 | 878 | 30 | 2.3% |

**Train-type structure by era (**purchase mix, not frequency**)**:

| Era | Train-type distribution (by count) |
|:--|:--|
| A | **2-train 2648** \| 2+2-train 1353 \| 3-train 314 |
| B | **4-train 1450** \| 3+3-train 1223 \| 3-train 1137 \| 2-train 571 \| 4+4-train 338 \| 2+2-train 153 |
| C | **6-train 1883** \| **6E-train 1458** \| 4+4-train 1357 \| **2R-train 682** \| 4-train 366 \| 8-train 245 \| 3+3-train 214 \| 3-train 210 |
| D | **2R-train 444** \| 8E-train 429 \| 10-train 283 \| 8-train 221 \| 6-train 139 \| 6E-train 127 \| 4+4-train 88 |

**Distribution of par prices by era**:

| Era | par=70 | par=80 | par=90 | par=100 |
|:--|--:|--:|--:|--:|
| A | 283 | 323 | 467 | **1109** |
| B | 69 | 309 | **564** | 179 |
| C | 88 | **341** | 226 | 58 |
| D | 4 | 18 | 7 | 1 |

> **The three-sentence summary of the era rhythm**:
> ① **Era A is "grabbing companies"**: par=100 holds an absolute majority (1109/2182 = 50.8%), buying the 2-train dominates, and a 98.2% payout drives the share price up.
> ② **Era B is "track laying and expansion"**: track lays double (11205), share purchases break ten thousand, par shifts towards 90/80, and companies begin to extend towards long-range routes.
> ③ **Era C is "hoarding cash and grabbing trains"**: the withholding rate rises to 30%, share sells double (2263), and the 2R "Restored 2-train" starts to appear (682 of them — **this is the key move that preserves running capacity for era D**).
> ④ **Era D is "harvest and crisis"**: only 213 stations placed (there is no room left), 30 companies opened (nobody opens any more), revenue ¥300/train, but **contributions explode (see 7.4)**.

### 7.2 Train calendar (when each train type sells out / is removed)

| Train type | Price | Median OR of sell-out / removal | Range |
|:--|--:|--:|:--|
| 2 | 100 | **OR1** | 1–1 |
| 2+2 | 180 | **OR2** | 1–2 |
| 3 | 180 | **OR3** | 1–3 |
| 3+3 | 300 | **OR3** | 2–4 |
| 4 | 300 | **OR5** | 3–5 |
| 4+4 | 450 | **OR5** | 4–6 |
| 6 | 600 | **OR6** | 5–7 |
| 6E | 700 | **OR7** | 5–8 |
| 2R (Restored 2-train) | 250 | OR8 | 5–10 |
| 8 | 800 | **OR9** | 6–9 |
| 8E | 900 | **OR10** | 7–10 |
| 10 | 1000 | OR10 | 8–10 |

**How to use it**:

- **The first 3 rounds decide whether you can "keep up with the era"**: the 2-train is gone at **OR1** (2888 events), the 3-train at **OR3**. **Era A gives you only ~2 Operating Rounds** to buy your first train.
- **The real train-explosion point is OR5–OR7**: the 4-train (OR5) rusts the 2/2+2, the 6-train (OR6) rusts the 3-train, the 6E (OR7) rusts the 3+3 — **in these three rounds every company has to change trains**, and this is **the reason the withholding rate rises to 30%**.
- **The 8/8E sell out at OR9–OR10**, and at that point **any company that has not bought an 8-train goes straight into a Forced Train Purchase**.

### 7.3 The trade-off between "no train to run" and "buying a 2R" (**a correlation ≠ causation that must be spelled out**)

- **The 2R (Restored 2-train) appears 682 times in era C and 444 times in era D**: price ¥250, **does not rust, does not fulfil the obligation to own a train, limited to 1 per company**, and buying one does not advance the train-purchase marker.
- **Measured result (counter-intuitive, and in the opposite direction to the "insurance" intuition)**:

| Company group | Share with a Director contribution in era D |
|:--|:--|
| **Companies that bought a 2R in era C** (n=663) | **24.3%** (161 companies) |
| Companies that did not buy a 2R in era C (n=3383) | **13.4%** (453 companies) |

> **This must not be read as "buying a 2R is harmful"** — it is almost certainly **reverse causation**: **buying a 2R is the symptom of "this company has no money and cannot run"**, not the cause. A company that in era C cannot afford a 6E/8-train and can only spend ¥250 on a 2R will of course find it even harder to scrape together the money for an 8-train by era D.
>
> **The correct way to use it**: **treat the 2R as "stopping the bleeding" rather than as "insurance"** — it lets you **keep operating and taking revenue** without being forced into a Forced Train Purchase (avoiding the double penalty of "no train → revenue to zero + share price shifting left"), but **it will not make a company stronger**. A company with money should go straight to an 8-train.

### 7.4 Crisis calendar (**this title's most important "danger points"**)

> ⚠️ **Basis (user correction)**: the **absolute count** of contributions is inflated by "more companies in the later eras + OR10 having three sub-rounds". The table below also gives the **rate normalized by company-round** — **after normalization the crisis peak is actually at OR9 (0.061 per round), and OR10 instead falls back to 0.033** (because it has 3 sub-rounds, giving companies more chances to top up their trains).

| OR | Company-rounds | Contributions | **Contributions/company-round** | Contribution amount | Note |
|:--|--:|--:|--:|--:|:--|
| OR1–4 | 15,337 | 4 | 0.000 | ¥220 | almost never happens |
| OR5 | 3,874 | 55 | **0.014** | ¥4,135 | the 4-train (¥300) starts squeezing cash |
| OR6 | 4,703 | 101 | **0.021** | ¥14,571 | the 6-train (¥600) |
| OR7 | 3,121 | 96 | **0.031** | ¥18,450 | 6E (¥700) |
| **OR8** | 5,124 | **190** | **0.037** | ¥51,077 | the 8-train (¥800) enters |
| **OR9** | 3,297 | 200 | **0.061 (peak)** | ¥84,707 | the 8-train sells out and trailing companies are forced; **highest per-round risk** |
| OR10 | **10,640** | **354** | 0.033 | **¥201,039** | the last Operating Round (**3 sub-rounds**): **largest total (53.7% of it) but diluted per round** |
| Full period | 46,096 | 1000 | **0.022** | ¥374,199 | — |

> **This is the definition of a "crisis point" in this title**: **not bankruptcy (players in this title never go bankrupt), but "you are forced to use your own money to buy a train for the company in OR8–OR10"**.
> - **OR10 alone accounts for 35.4% of all contributions** (354/1000) and **53.7%** of the amount (¥201k/¥374k) — because that is the last round, and companies must own a train before the end.
> - **The average contribution is ¥270**, but the mean for OR10 is **¥568** — **a contribution in the final round is often the price of one 8-train in a single go**.
> - **Only 134 of them used a personal loan** (50% interest) — **which shows that the vast majority of Directors have cash on hand** (§6.1: in era D the median player cash is ¥1180). **"Holding a large amount of cash in era D" is not safety, but "about to be contributed out"**.

### 7.5 The practical meaning of the Communist Takeover period (B3–C2)

| Rule | Practical impact (measured) |
|:--|:--|
| The share price is **frozen** (it does not move in either the Stock Round or the Operating Round) | **Share sells in era C double (2263)** — everyone rushes to adjust their positions before/after the freeze; during the freeze **dumping does not work**, and neither can you push the share price up through dividends |
| **A Director may not sell shares of his own company** | The route of "sell off my own company for cash" is blocked off → **you must set cash aside in era B in advance** |
| Private Companies stop paying dividends (their abilities remain) | Private Companies change from "cash flow" into "pure ability tools"; **if P0's one-off payment was not collected before B2/B3, only the ¥100 tier remains by phase C** |
| Foreign Investors stop operating (mergers still happen) | Investor income in era C goes to zero → **the earlier the merger, the better** (§4.5: median merger at OR4) |

### 7.6 "Inter-company train transfer (same Director)": this title's real mainstream cash tool (**2930 times / 337 games = 8.6 per game**)

> **This section answers the user's question: why are train transfers so frequent? Is it a cash squeeze, or everyone having several companies, or buying trains faster to push the eras forward?** — **all three hypotheses were tested against the data, and the conclusion is that "cash squeeze" is the main cause, while the other two do not hold**.

Rule §X: **after the first 3-train has been bought (Phase B1)**, a company may buy trains from another company **of the same Director**, at **a free price, with a floor of ¥1, and the deal must be public**.

| Item | Data |
|:--|:--|
| Number of transfers | **2930 times / 337 games = 8.59 per game** (for comparison: 34,784 tile lays and 9,270 station markers placed across the whole title, i.e. 102 lays／27 markers per game — **the volume of transfers is equivalent to "2 per player per game"**) |
| Train types | 2-train 571 \| 4-train 409 \| 6-train 388 \| 4+4-train 339 \| 3-train 324 \| 3+3-train 258 \| 6E-train 216 \| 2R 198 \| 2+2 153 \| 8-train 36 \| 8E 24 \| 10-train 14 |
| **Price** | median **¥110**, mean ¥181, range ¥1–¥1470 |
| **¥1 deals** | **652 (22.3%)**; **≤¥5 in total 712 (24.3%)** |
| By era | **C 1474 \| B 925 \| D 531** |
| By OR | OR2 184 \| **OR6 542 (peak)** \| OR9 219 \| OR10 288 |

> **This is a severely underrated mechanism in this title**: **a train transfer = the same Director "moving money" between several of his own companies**.
> - **A ¥1 transfer (22.3%) is essentially a free transfer** — moving a train (and the operating capacity it brings) from one company to another, in order to **circumvent the restriction that "company treasuries may not lend to one another"**.
> - **The peak is at OR6** (542): exactly the round in which the 6-train (¥600) enters and the era C withholding rate soars to 30% — **a rich company turns money into a train and passes it to a poor company, which is equivalent to giving the poor company a transfusion**.
> - **This explains the puzzle in §6.4**: why are there 1000 contributions yet only 134 personal loans? Because **a Director can first solve the problem with his own companies' money**; only when "several of his own companies are short of money at the same time" does he have to touch his personal cash.

#### 7.6.1 Why are train transfers so frequent? — **four hypotheses tested one by one**

| # | Hypothesis | Test method | Result | Verdict |
|:--|:--|:--|:--|:--|
| **A** | **Because of a "cash squeeze"** (the buyer cannot afford that era's new train) | The share of transfers in which the buying company's treasury ≥ the era's new-train price (A ¥100 / B ¥300 / C ¥600 / D ¥800) | **only 35.3% (1035/2930)** — i.e. **64.7% of transfers happen when "the buyer cannot afford a new train"** | ✅ **holds; it is the main cause** |
| **B** | Because "everyone has several companies" | The distribution of "number of companies under the Director" in transfer events vs the baseline distribution over all company-rounds | Transfers: 1 company 2.8%／2 companies 25.9%／3 companies 42.8%／4 companies 21.3%／5+ 7.2%<br>Baseline: 1 company 3.9%／2 companies 27.8%／**3 companies 41.8%**／4 companies 20.1%／5+ 6.4% | ❌ **does not hold** — **the two distributions are almost exactly the same**, showing that transfers occur **proportionally across company-rounds** (with only 1 company a transfer is physically impossible, 0.2%, which follows necessarily from the rules) |
| **C** | In order to "buy trains faster and push the eras forward faster" | The correlation between the number of transfers and the OR index at which era D is entered | **Pearson r = −0.006 (n=341)**; games with ≤4 transfers enter D at a median of **OR 8.5**, games with ≥13 transfers at **OR 9.0** | ❌ **does not hold** — **no correlation at all, and the direction is even reversed** (games with more transfers enter D later). What pushes the eras forward is **buying new trains from the bank**; transfers do not advance the train-purchase marker |
| **D** | Because "old trains are about to rust, so shuffle them in advance" | The train-type distribution of transfers vs the rust table | The train types are dominated by **2-train 571, 4-train 409, 6-train 388** — all **old trains already at or near their rust point**; by era the distribution is **C 1474 \| B 925 \| D 531** | ✅ holds (complementary to A: **the old train is the chip, the poor company is the recipient**) |

**Conclusion: frequent transfers = the superposition of "cannot afford a new train" + "old trains have residual value to be used"**, unrelated to "number of companies" and unrelated to "accelerating the eras".

#### 7.6.2 The transfer-channel structure (who transfers to whom)

| Company pair | Count | Geographic meaning |
|:--|--:|:--|
| **BCR → CKR** | **184** | Chengdu → Kunming (the Chengkun line, the thickest sibling channel in this title) |
| JHA → BZU | 73 | Beijing → Harbin (the Jingha line) |
| JHU → JHA | 69 | inside Beijing (Jinghu → Jingha) |
| HKR → NJR | 65 | Shanghai → Nanjing (Huning) |
| HKR → SCR | 64 | Shanghai → Changsha (the Hukun line) |
| JHU → JGG | 63 | inside Beijing |
| BCR → SCR | 61 | Chengdu → Changsha |
| BCR → WNR | 40 | Chengdu → Wuhan |

> **Train transfers almost only happen between companies that are "geographically adjacent"** — because **companies under the same Director expand in the same region** (the station placement in §7.7 follows the same logic). **BCR→CKR at 184 is the number-one channel in the whole title**, and these are precisely the two ends of the Chengkun line.

**Three conclusions for players**:

1. **Train transfers are routine play, not a trick** (8.6 per game): **as long as you are the Director of 2 or more companies, you should by default transfer old trains at ¥1–¥300 to whichever of your own companies lacks a train**, rather than letting the poor company carry the new-train price + a Director contribution.
2. **A ¥1 transfer (22.3%) is the cleanest kind of "inter-company transfusion"**: turning a rich company's cash into an old train and passing it to a poor company **avoids both the 50%-interest personal loan and any need for the Director to pay out of his own pocket** (this is exactly why there are only 134 personal loans in §6.5).
3. **Transfers do not accelerate the era, but they do accelerate "the renewal of your own fleet"**: a transfer does not advance the train-purchase marker (§X), so it **only changes the distribution of running capacity inside your group, and does not change the pace of the table** — to push the era you still have to buy a new train from the bank.

> **Examples**: `1880-132` (**35 transfers** in a single game), `1880-093` (29), `1880-107` (26); typical **¥1 transfers**: `1880-001` (SCR transfers a 2R out for ¥1 in OR6), `1880-002` (NJR transfers a 2-train out for ¥1 in OR2).

### 7.7 Where to place the station: **placement decides fate** (**Question 3**)

**Totals**: across all 341 games, **9270** station markers were placed (= 27.2 per game, including each company's free home station). Each company has a fixed **3** (home station + 2 extra; extra stations ¥40/¥100, rising to ¥80/¥200 in Phase D, §X).

**① Placement frequency (home stations included) Top 20**

| Hex | City | Count | Share | Mean cost |
|:--|:--|--:|--:|--:|
| **F8** | **Beijing** | **1834** | **19.8%** | ¥21 |
| N12 | Changsha (Changsha & Nanchang) | 605 | 6.5% | ¥12 |
| **K15** | **Shanghai** | **605** | 6.5% | ¥31 |
| D12 | Shenyang (Shenyang & Fushun) | 519 | 5.6% | ¥15 |
| **F10** | **Tianjin** | **517** | 5.6% | ¥58 |
| K13 | Nanjing | 493 | 5.3% | ¥23 |
| M3 | Chengdu | 483 | 5.2% | ¥18 |
| L10 | Wuhan | 384 | 4.1% | ¥20 |
| O5 | Kunming | 370 | 4.0% | ¥11 |
| **L14** | (unnamed town hex, Shanghai–Changsha corridor) | **310** | 3.3% | ¥50 |
| M7 | Chongqing | 304 | 3.3% | ¥50 |
| B8 | Harbin | 297 | 3.2% | ¥8 |
| H10 | Jinan | 281 | 3.0% | ¥64 |
| **E9** | (unnamed town hex, Beijing–Tianjin corridor) | 211 | 2.3% | ¥52 |
| I9 | Kaifeng & Zhengzhou | 201 | 2.2% | ¥66 |
| C11 | Jilin City | 185 | 2.0% | ¥52 |
| H14 | Qingdao | 176 | 1.9% | ¥67 |
| J6 | Xi'an | 171 | 1.8% | ¥12 |
| H6 | Taiyuan | 170 | 1.8% | ¥58 |
| P12 | Macau | 136 | 1.5% | ¥59 |

> **Beijing's 19.8% is structural**: the **home stations of all four companies JHA/JHU/JGG/JLR are in Beijing** (F8), and every game in this title has 4 Beijing companies. **Setting the home stations aside, the real "must-contest points" are Tianjin F10 (517 times) and Shanghai K15 (605 times)**.

**② Placement → Director outcome (only the extra 2nd/3rd markers, n≥25)**

| Placement | n | **Director top-two rate** | Mean rank per game |
|:--|--:|--:|--:|
| **Tianjin** | 517 | **60.3%** | 2.44 |
| **L14 (Shanghai–Changsha corridor)** | 310 | **60.0%** | 2.34 |
| Shenyang & Fushun | 108 | 54.6% | 2.58 |
| **E9 (Beijing–Tianjin corridor)** | 211 | 54.5% | 2.49 |
| Shanghai | 267 | 53.9% | 2.50 |
| Macau | 136 | 52.9% | 2.57 |
| Kunming / N10 / H8 / Changsha / Chengdu | 53–142 | 51.4–52.8% | 2.47–2.72 |
| … | | | |
| **Kaifeng & Zhengzhou** | 201 | **39.3%** | 2.95 |
| **Dalian** | 115 | **39.1%** | 3.01 |
| **Jilin City** | 185 | **38.4%** | 2.98 |

**③ Station preferences of TOP players (n≥8 games, BT≥2.0) (share difference vs the remaining players)**

| Placement | TOP share | Others' share | Diff |
|:--|--:|--:|--:|
| **L14** | **12.4%** | 5.4% | **+7.0pp** |
| **Macau** | **7.7%** | 2.1% | **+5.6pp** |
| E9 | 7.2% | 3.9% | +3.3pp |
| Chengdu | 5.2% | 2.6% | +2.7pp |
| Nanning | 4.1% | 1.8% | +2.3pp |
| Shanghai | 7.0% | 5.2% | +1.8pp |
| Tianjin | 11.4% | 10.4% | +1.0pp |
| **Beijing** | 9.0% | 10.5% | **−1.6pp** |
| **Chongqing** | 3.9% | 6.5% | **−2.6pp** |

> **TOP's station philosophy is very clear**: **place more at L14 / Macau / E9 / Chengdu (revenue-type corridors), place fewer at Beijing / Chongqing (congested hubs)**. Although Beijing has the most placements (19.8%), **that is forced by the four companies' home stations** — TOP clearly avoids it for "extra stations" (−1.6pp).

**④ Private Company holder → placement (**what Question 3 asks for — "combined with Privates"**)**

| Private Company (ability) | The stations most often placed by the company of that company's Director |
|:--|:--|
| **P3 Taiwan Western Line** (Taiwan +20) | **L14×162**, Beijing×134, Tianjin×106, Shanghai×93, Chongqing×68 |
| **P4 Chinese Rivers Ferry Companies** (lay track on rivers −¥20) | **Tianjin×126**, Beijing×121, **Chongqing×96**, L14×85, Shanghai×75 |
| **P5 Jeme Tien Yow Engineer Office** (Phase D permit) | Tianjin×163, Beijing×150, L14×61, **Jilin City×60**, Jinan×53 |
| **P2 Yanda Ferry Company** (free ferry) | Beijing×139, Tianjin×125, Shanghai×92, **Qingdao×82**, Jinan×80 |
| **P7 Rocket of China** (inter-company train transfer) | Tianjin×151, Beijing×120, Jinan×78, E9×71, L14×58 |

**⑤ Foreign Investor holder → placement (**what Question 3 asks for — "combined with Foreign Investors"**)**

| Investor (Home Base) | The holder's company's top three placements | **Does it agree with §4.3's "investor → associated company"?** |
|:--|:--|:--|
| **French** (French Indochina Q7) | **Chongqing×196**, Beijing×89, Chengdu×63 | ✅ **a complete match** — Chongqing is the unavoidable point for BCR (Chengdu) to exit eastwards; French Investor → BCR 243 times |
| **British** (Hong Kong Q15) | **L14×123**, Beijing×93, Tianjin×83 | ✅ L14 is the Shanghai–Changsha corridor; British Investor → HKR 114 times |
| **Portuguese** (Macau P12) | Beijing×104, **L14×83**, Tianjin×69 | ✅ Portuguese Investor → HKR 84 times, taking the same Shanghai corridor |
| **Russian** (Russia A3) | **Jilin City×93**, **E9×85**, Tianjin×74 | ✅ Russian Investor → JHA 96 times, taking the northern (Harbin–Beijing) corridor |
| **Belgian** (Taiyuan H6) | Tianjin×85, **Taiyuan×57**, Beijing×55 | ✅ Belgian Investor → JGG 53 times; Taiyuan is the home base |
| **Japanese** (Dalian E13) | Tianjin×51, **Dalian×25**, Beijing×25 | ✅ Japanese Investor → JHU 38 times; Dalian is the home base |
| **German** (Qingdao H14) | Tianjin×102, Beijing×69, Shanghai×50 | ✅ German Investor → JHU 67 times / HKR 61 times |

> **④⑤ are cross-corroboration**: **station placement is not randomly distributed** — **whichever corridor a Private Company's ability points to, its holder puts the stations there; whichever company an investor's Home Base connects to, its holder lays in that direction**. This consistency shows that this title's "Private + Investor + Company + Station" is a **mutually interlocking system**, not four independent choices.

---

## 8. TOP Players' Play (**Question 3**)

### 8.1 Three-tier behaviour comparison (weighting: top 3 / mid 2 / bot 1, converted into a "per-player-game average")

| Metric | **top** (8 players, 200 games) | mid (13 players, 231 games) | bot (15 players, 195 games) | Reading |
|:--|--:|--:|--:|:--|
| Private spending ÷ starting capital | 39.9% | 37.0% | 38.5% | **All three tiers are almost the same** — Privates are not the key to tiering |
| Companies founded | 2.90 | 2.82 | 2.95 | No difference |
| SR1 companies founded | 0.8 | 0.8 | 0.9 | No difference |
| par=100 count | 0.9 | 1.0 | 0.9 | No difference |
| Share purchases | 19.3 | 17.3 | 17.9 | Slightly higher |
| **Share sales** | **4.2** | 2.8 | **2.5** | **top is 1.7× bot** |
| **Share-sale income** | **¥564** | ¥339 | ¥300 | **top is 1.9× bot** |
| Personal loans | 0.04 | 0.06 | 0.07 | All very low |
| **Director contribution amount** | **¥79** | ¥111 | **¥250** | **bot is 3.2× top** |
| Endgame holdings in own controlled companies | 165% | 173% | 187% | Slightly lower |
| Endgame total holdings | 201% | 204% | 215% | Slightly lower |
| Endgame cash | ¥4493 | ¥4245 | ¥4403 | No difference |
| **Investor preference** | **Portuguese 34%, French 26%, British 21%** | French 22%, German 16%, Portuguese 16% | British 24%, French 22%, Portuguese 19%, **German 17%** | **top clearly favours Portuguese/French/British and avoids German/Japanese** |

### 8.2 TOP's three replicable traits

**① Share-selling frequency is 1.7× that of ordinary players (4.2 times/game vs 2.5, income ¥564 vs ¥300)**

TOP does not "hold its shares and sit still", but **continuously rotates its positions**: it sells off the shares of companies that have already risen to their target (§XIII: selling shares moves the share price down one space, but **this title has no Bank Pool — shares sold go straight back to the Bank**, so **you can buy them back at any time**), and directs the cash into the next company about to float.
Mechanically this makes sense: **the only frictionless arbitrage path in 1880 is "sell mature companies with a high par, buy new companies with a low par"** — because **new shares have the lowest holding cost in SR1–SR3, while mature companies have already risen to 150–200, so the marginal return of continuing to hold (the dividend/price gain per 10% share) is lower than that of switching positions**.

**② Director contribution amount is only one third of bot's (¥79 vs ¥250)**

TOP is rarely emptied out by a Forced Train Purchase in era D — because they **stockpiled enough money to buy trains for their own companies already in era C** (the window with a 30% withholding rate), and **had their companies buy an 8-train or a 2R before OR8**. **A director contribution is the fine for "not preparing in advance".**

**③ Picking investors carries a clear preference: Portuguese / British / French (81% in total), avoiding German / Japanese**

Consistent with the results of §4.2: the Portuguese (Macau) and British (Hong Kong) Home Bases are on the **southeast coast** and connect fastest to **HKR (Shanghai)**; the French (French Indochina) **lock in BCR**. TOP players **do not pick at random or in sequence, but by "can it quickly connect to the company where I am Director"**.

### 8.3 The highest-win-rate play (four data-supported items, ordered by strength of evidence)

| # | Play | Evidence |
|:--|:--|:--|
| **1** | **Be the Director of 4–5 companies** | Top-two rate **61.8% (4 companies) / 65.3% (5 companies)**, versus only **18.4%** for 1 company (§5.2). **This is the strongest monotonic relationship in the whole title** (n=266) |
| **2** | **Grab HKR (Shanghai) in SR1** | The top-two rate of Directors who found HKR in SR1 is **59.9%** (184/308), and it is the best opening in 89.1% of games (§5.4) |
| **3** | **Buy 3–4 Private Companies, but keep enough left to found companies** | The more held the better (**4 privates 55.8%** vs 0 privates 41.7%, §3.5); the top-two rate of those who buy **P3 (Taiwan Western Line)** is **59.5%** (§3.6); **the only forbidden zone is "spending down to zero cash" (>83% of funds → 34.2%)** |
| **4** | **Found weak companies late (SR5+), strong companies early** | QSR founded late **65%** vs early 34%; JLR 60% vs 38%; JGG 61% vs 37%; HKR the reverse (§5.6) |

**Negative list (avoid these)**:

| Play | Top-two rate | Note |
|:--|--:|:--|
| Be the Director of only 1 company | 18.4% | Equivalent to handing the decision-making power to others |
| Private spending > 83% of starting capital (>¥400) | 34.8% | Founding directly in SR1 becomes impossible |
| Found JGG (Jingguang, purple) in SR1 | 36.2% | The four Beijing companies undercut each other; JGG suffers the most |
| Found QSR / JLR / WNR in SR1–2 | 33–38% | Founding a weak company early = buying a weak asset at a high price |
| Buy a single P5 (Jeme Tien Yow Engineer Office) | 41.1% | A permit that only takes effect in Phase D, arriving too late |
| Only think of saving money for your company in OR8–10 | Average contribution ¥568 each time | See §7.4 the crisis calendar |

### 8.4 A counter-intuitive contrast: **TOP is not "saving money"**

In §8.1, the three tiers' **private-spending ratios (39.9% / 37.0% / 38.5%), companies founded (2.90 / 2.82 / 2.95) and share purchases (19.3 / 17.3 / 17.9) are almost exactly the same**. **TOP's advantage is not in "doing more", but in "turnover"**:

- **The same input, but top's selling frequency is 1.7× higher** (the same money turns over more times in top's hands);
- **The same number of companies founded, but the companies top founds are less often driven into a contribution** (¥79 vs ¥250).

> In one sentence: **the winner at 1880 is not the one who is "more aggressive" or "more conservative", but the one who "turns over cash flow faster and prepares more fully before a crisis".**

---

## 9. Actionable Conclusions — Quick Reference (Table-Side Version)

### Opening (auction + SR1)

1. **Buy 3–4 Private Companies** (top-two rate 50.0% / 55.8%, versus only 41.7% for 0); **the one forbidden zone is spending down to zero cash** (spending > 83% of starting capital → the top-two rate drops to 34.2%, and in that band the median number of companies founded in SR1 is 0). **After buying you must still have enough left to found a company.**
2. **First choice P3 (Taiwan Western Line, ¥45, buyers' top-two rate 59.5%)**; second choice the P0+P6 combination (58.1%) or P4+P6 (55.6%).
3. **P6 (Imperial Qing Government, ¥160–245) is the best-value buy of all**: it = **the BCR Director's Certificate + first pick of Foreign Investor** (341 games, zero exceptions). **If you win P6, do not hesitate — take BCR into your own hands.**
4. **Deals at the minimum bid (bid premium = 0) are only 33 auctions / 2728 = 1.2%**, and **they all happen in cold auctions with only 1 bidder** (mostly 3-player games); **as long as 2 or more players want it, the bid premium is never 0**. **P0 is the only company that never once sold at the minimum bid in 341 games** (median bid multiple **5.00×**: face value ¥5 → median winning bid ¥30). Across the board the median bid premium is **+¥25**, p90 +¥50; **the lowest bid multiple is P5 (0.25×), the highest are P0 (5×) and P7 (0.9×)**.
5. **In SR1 found only HKR (Shanghai)** (founded in 89.1% of games, Director's top-two rate 59.9%); **JHU / JHA / NJR / SCR / CKR are acceptable choices**; **in SR1 do not touch JGG / QSR / JLR / WNR / LHR / NXR**.

### Taking Shares (par and certificates)

6. **The standard opening: par=100 + a 20% Director's Certificate + an ABC three-phase Building Permit** (¥200 to start, track-laying rights covering A/B/C) — this is the mainstream configuration, used 928 times across the title.
7. **A late-founding company (round 5 onward): par=70–80 + a 40% certificate** (¥280–320 in one go, no need to build the stake up further, and a weak company founded late has a top-two rate of 60%+).
8. **Companies with par=100 have a median endgame Director holding of 70%** (grab the company first, hold the stake later); companies with par≤90 end at only 50%.

### Operations (every round)

9. **In era A, 98% payout** (push the share price + give players cash); **in era C, 30% withholding** (save up for the 6/8-train); **in era D, straight back to 98% payout**.
10. **Train calendar**: 2-train OR1 sold out|3-train OR3|4-train OR5|6-train OR6|6E OR7|8-train OR9|8E OR10.
11. **Before OR8 you must have ready, for every company, the money to buy an 8-train** (¥800) — otherwise the director contributions of OR8–10 will drain you personally dry (OR10 averages **¥568** per occurrence).
12. **2R (¥250) is a styptic, not a tonic**: it keeps a company operating, but it does not change strong from weak (a company that buys 2R is in fact more likely to need director contributions — reverse causation).

### Investors

13. **Pick your Foreign Investor by "can it connect to the company where I am Director"**: Portuguese (Macau) / British (Hong Kong) → HKR (Shanghai); French (French Indochina) → BCR (Chengdu); Russian / German / Belgian → the four Beijing companies.
14. **At a merge choose "transfer the whole amount to the company"** (measured: 95.2% of players do this; taking 20% has a median of only ¥40).
15. **99.4% of investors merge before the game ends** — do not count on "taking 20% at the finish".

### Crisis and Endgame

16. **Players in this title never go bankrupt**, but there are **personal loans at 50% interest** (134 instances / 0.39 per game). **Loan-takers' top-two rate is only 19.8%** (versus 48.4% for those who only make director contributions) — **taking a personal loan = already on the losing path**.
17. **During the Communist Takeover period (B3–C2) you cannot sell shares in your own company** — to rebalance your position during the freeze, **you must act in advance, in era B**.
18. **The end point is fixed**: after the second 8-train every company operates 3 more times (**100% of games run to D2/D3, and OR is always 10**) — **the last three rounds are the settlement round of "who still has trains, who still has money"**.
19. **The median gap between champion and runner-up is only ¥521** — this title is extremely tight, **any single payout or director contribution in the last three rounds can flip the game**.

### Corporate Governance and Stations (new)

20. **Change of Director is 0.41 times per game**, and **79.7% are "the incumbent Director sold it himself"** (these 110 players' top-two rate is **56.4%**, above the baseline — **selling a mature company to reposition gives a positive return**); **only 20.3% are "bought out by someone else"**, and these 28 former Directors' top-two rate is only **28.6%**. **When someone builds a stake in your company, "ignoring it" is clearly negative EV.**
21. **Put stations at these three places first**: **Tianjin (Director's top-two rate 60.3%), the L14 Shanghai–Changsha corridor cell (60.0%), the E9 Beijing–Tianjin corridor cell (54.5%)**; **avoid Jilin City (38.4%), Dalian (39.1%), Kaifeng & Zhengzhou (39.3%)**. **TOP players place at L14 2.3 times as often as ordinary players** (12.4% vs 5.4%), while **Beijing, though stations land there most often (19.8%), is precisely what TOP avoids** (the four companies' home stations are simply forced there, −1.6pp).
22. **Stations should follow "Private Company ability + Foreign Investor Home Base"**: P3 Taiwan → the Shanghai–Changsha corridor (L14); P4 rivers → Chongqing; French capital → Chongqing (connecting BCR); British / Portuguese capital → L14 (connecting HKR); Russian capital → Jilin / E9 (connecting JHA). **The four choices are one system, not four separate things.**

---

## 10. Method and Limitations

### 10.1 Toolchain (all in `_tools/`, reproducible)

| Script | Role |
|:--|:--|
| `split_1880_cases.py` | split / de-duplicate / canonical numbering (384 → 358 games, outputs `_work/1880_cases/_mapping.csv`) |
| **`parse_1880_log.py`** | **reconciliation state machine** (player-by-player net worth vs the official settlement, **1564/1564 = 100.00%**) |
| `extract_1880.py` | batch extraction → `_work/1880_full.json` (358 games, 41 MB) |
| `rating2_1880.py` | top-two rank rating + Bradley-Terry → `_work/1880_rating.json` (517 players) |
| `analyze_1880.py` | subcommands `overview` \| `auction` \| `order` \| `founding` \| `tiers` \| `epoch` \| `lobby` |
| `stats2_1880.py` | subcommands `sr1` \| `holdings` \| `combo` \| `rush` \| `topstyle` \| `corpfin` |
| `_render1880.py` / `_render1880_clip.py` | high-magnification page-by-page / by-region rendering of the PDF (for checking the rulebook tables) |

**Every number in this report can be traced back to a stretch of output from one of the scripts above.**

### 10.2 Cross-validation

| Check | Result |
|:--|:--|
| Net worth reconciliation | **1564/1564 = 100.00%**, residual 0 |
| Splitting correctness | all 358 games contain `-- Game over:` and contain `-- Phase A1`, 1:1 pairing |
| Opening-bidder rotation formula | 2685 auctions **98.4% match** (the exceptions are re-auctioned lots) |
| Priority Deal order = draft order | **341/341 = 100%** |
| Rulebook table figures | the phase table / company table / Private Company table were all **visually checked** back against the original book pages, and cross-validated as consistent with the 18xx.games open-source implementation `g_1880` |

### 10.3 Six limitations (**read before citing this report**)

1. **The sample is dominated by 4-player games (46.9%)**; 3-player 16.1%, 6–7-player games together only 9.4% — **the specific figures for 6/7-player games are for reference only**.
2. **Of 517 players, 448 have fewer than 5 games** — the strength table and the tiers hold only for **the 36 players with n≥8 games**; **the behavioural differences of tiered players cannot be extrapolated to "all strong players"**.
3. **Tier comparisons are descriptive, not causal**: the top group's "sells more shares, contributes less" may mean **they got it right**, or it may mean **they were ahead, so they had room to manoeuvre**. This report sets out the causation warning separately at §5.2, §5.6 and §7.3.
4. **"A company founded late has a top-two rate of 60%+" suffers from a severe selection effect**: players who can wait until SR5 to found a company are often **already ahead to begin with** (they have cash in hand). **Do not take it as a reason to "do nothing in the early game"**.
5. **The familiar-table sample is only 56 games** (only 42 games at ≥4 players) — this title's data is **not enough to reproduce** 1846's "familiar tables compress the skill gap" conclusion, and this report does not draw that conclusion.
6. **Transferability of the company-strength conclusions**: this title **has no random attributes** (§0.1), so §5's company conclusions **carry over across games**; but **players' meta keeps evolving** — this dataset covers public games on 18xx.games from 2023–2026, and future shifts in table style are not in the sample.

---

## Appendix

### A. Mapping onto the 1846／1830／1862 report frameworks

| 1846's topic | What it corresponds to in 1880 | Where in this report |
|:--|:--|:--|
| Private Company pairing | **Private Company combinations** (P0+P6 58.1%, P2+P3 69.2%) | §3.6 |
| Number of stations and first claim | a fixed 3 per company; station cost ¥40/¥100 (doubled in Phase D) | §2.5, §7.1 |
| Train-rush cadence | the **train calendar** (2-train OR1 … 8E OR10) | §7.2 |
| President fronting money | **director contribution 2.93 times per game**, OR10 accounting for 35% | §6.4, §7.4 |
| Inter-company train transfer | in this title **companies with the same Director may transfer trains** (from Phase B1), **measured at 8.59 times per game** — the main cash-scheduling tool in this title | **§7.6** |
| E/W bonus routes | **this title has no E/W bonus**; in its place are the **share-price table bonus + the Trans-Siberian Railway bonus (¥50)** | §2.3 |
| The par-price sweet spot | **par=100 + a 20% certificate** is the standard solution; **for late founding use par=70–80 + 40%** | §5.3 |
| Bankruptcy | **this title has no bankruptcy**; the form the crisis takes is **director contribution** | §7.4 |

### B. Topics this report does not cover that are worth adding in the next version

1. **Network and hub centrality** (the approach taken in the 1830 report, §11–§12): this title's map (`1880ChinaMap.png`) has not yet been grid-calibrated or put through betweenness analysis — **"which corridor is a route one must pass through" can directly explain why HKR/BCR are strong**.
2. **A finer-grained breakdown of inter-company train transfers**: this report has already given the totals (**2930 instances / 8.59 per game / 22.3% at ¥1**), but **"who transferred a train to whom, and how the two companies' treasuries changed before and after the transfer"** has not yet been modelled transfer by transfer, and is worth a chapter of its own.
3. **An analysis of the timing of P7 Rocket of China's use** (**357 exchanges** across the 341 games, see the train-type table in §5.1; the exchange timing and the revenue they yield are worth a section of their own).
4. **A dedicated chapter on 7-player games** (this title supports up to 7 players, and the certificate limit drops to 11 — the only player count at which "the limit really does constrain people").

---

*Acknowledgement: all game data comes from [18xx.games](https://18xx.games/) and the players who published their games there for free.*

*Analyst: Pig9013 | Model used: Deepseek-flash | Date compiled: 2026-09-22*
