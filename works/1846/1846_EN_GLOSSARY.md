# 1846 Report — Chinese→English Translation Glossary
> 🔒 **Player anonymisation (2026-09-24)**: the TOP group keeps real names; **all MID/BOT players are rendered as `PlayerX`** (numbered by strength descending, MID band first, then BOT).
> **Acknowledgement**: All game data comes from **[18xx.games](https://18xx.games/)** and the players who play there — it is their freely published play records that made this analysis possible.

Binding for the translation of `1846深度分析（672局）.md`.
Derived from `18XX_中英术语对照表.md` (§1 core terms, §3.2 1846-specific terms).

## 1. Rule terms (from the official English rulebook — these are the *original* words, not translations)

| 中文（报告用词） | English (authoritative) |
|:--|:--|
| 股份 / 股 | share |
| 证书 | certificate |
| 总裁 | president |
| 总裁证书 | President's Certificate |
| 公司金库 / 金库 | treasury |
| 银行池 | Bank Pool |
| 公开市场 | Open Market |
| 招股价 / par | par price |
| 初始股价 | initial share price |
| 股价 | share price / market price |
| 上市 / 发起 | float / found |
| 车站标记 / 车站 | station marker |
| 母城 / 主场所 | home city |
| 股票轮 (SR) | Stock Round (SR) |
| 营运轮 (OR) | Operating Round (OR) |
| 阶段 (Phase I–IV) | phase |
| 优先交易卡 | Priority Deal |
| 铺设轨道 / 铺轨 | lay track |
| 升级 | upgrade |
| 板块 | tile |
| 大城市 / 小城市 / 小镇 | large city / small city / town |
| 路线 | route |
| 封锁 | blocking |
| 火车 / 车 | train |
| 车型 | train type |
| 2 车 / 3/5 车 / 4/6 车 / 7/8 车 | 2-train / 3/5-train / 4/6-train / 7/8-train |
| 报废 | rust |
| 过时 | obsolete |
| 车辆上限 | train limit |
| 弃置 | discard |
| 运行火车 / 跑车 | run trains |
| 收益 | revenue |
| 股息 / 分红 | dividend |
| 留存 | withhold |
| 派息 / 全额派息 | payout / full payout |
| 半额股息 | half dividend |
| 强制购车 | Forced Train Purchase |
| 紧急筹资 | Emergency Money Raising |
| 增发 | issue (shares) |
| 赎回 | redeem |
| 淘汰 | phased out |
| 移除 | removed |
| 净资产 | net worth |
| 玩家财富 | player wealth |
| 标准轨 | standard gauge |

## 2. 1846-specific terms (§3.2.2 of the glossary)

| 中文 | English |
|:--|:--|
| 私有公司 / 私营公司 | Private Company |
| 独立铁路 / 小公司 | Independent Railroad |
| 标价 | List Price |
| 土地拨赠红利 | Land Grant Bonus |
| 东西向路线 (E/W) | East-West Route |
| 铁路关闭 | closing a railroad |
| 汽船公司 | Steamboat Company |
| 邮政合同 | Mail Contract |
| 新兴城镇 | Boomtown |
| 肉类加工公司 | Meat Packing Co. |
| 隧道爆破公司 | Tunnel Blasting Company |
| 芝加哥与西印第安纳 (C&WI) | Chicago & Western Indiana (C&WI) |
| 湖滨线 | Lake Shore Line |
| 小迈阿密 | Little Miami |
| 密歇根南方 (MS) | Michigan Southern (MS) |
| 密歇根中央 | Michigan Central |
| Big 4 | Big 4 |
| 俄亥俄与印第安纳 | Ohio & Indiana |
| 公司（七家） | IC, NYC, B&O, GT, PRR, ERIE, C&O |

## 3. Report-specific analytical vocabulary (coinages — one English rendering each, used consistently)

| 中文 | English | Notes |
|:--|:--|:--|
| 前二制 rank rating | top-two rank rating | 1st +2, 2nd +1, 3rd onward −(rank−2) |
| Bradley-Terry 实力分 | Bradley-Terry (BT) rating | |
| 归一化名次 | normalized rank | (rank−1)/(players−1) |
| 前二率 | top-two rate | share of games finished 1st or 2nd |
| 夺冠率 | win rate | |
| 残差 | residual | |
| 水平校正后的残差 | skill-adjusted residual | |
| 净效应 | net effect | doer's residual − non-doer's residual |
| 分层（TOP/MID/BOT） | tier (TOP/MID/BOT) | |
| 熟人局 | familiar table | all pairs have met ≥2 times |
| 路人局 | stranger table | no pair has ever met |
| 混合局 | mixed table | |
| 顺位 | turn order / position | |
| 招股价分布 | par distribution | |
| 甜点区 | sweet spot | |
| 首车组合 | opening train combination | |
| 首车时点 | timing of first train purchase | |
| 爆车 | rust event | |
| 爆车当口 | at the moment of rusting | |
| 爆到无车 | rusted down to zero trains | |
| 垫钱 | director contribution | |
| 倒车 | inter-company train transfer | |
| 皮包公司 / 空转公司 | shell corporation | |
| 爆破 | deliberate blow-up | §17 |
| 砸价 | price dumping | §12.4 |
| 超发 | share issuance from treasury | |
| 提升度 | lift | P(company\|private) / P(company) |
| 派息滞后 | payout lag | |
| 窗口收入 | window revenue | OR1.1–OR2.2 |
| 后发公司 | late-founded company | |
| 咽喉 | chokepoint | |
| 落站 | placing a station marker | |
| 首占 | first to occupy | |
| 路线覆盖率 | route coverage | |
| 残差总表 | residual master table | |
| 可执行结论速查 | actionable conclusions — quick reference | |

## 4. Style rules

1. **Literal translation.** No summarizing, no merging, no reordering. Every table row, every
   footnote, every caveat survives.
2. **All numbers verbatim** — same digits, same currency symbol (¥ → $), same percentages.
3. **Markdown structure preserved** — same heading levels, same table shapes, same emphasis.
4. **Chinese book quotes 「」→ English double quotes.** Full-width colon → colon + space.
5. Player handles (Player16, Player12, Player6…) and company abbreviations (IC, NYC…) are **not translated**.
6. Section labels §0–§21 keep their numbering; "〇" → "0".
7. Where the report cites a rule, keep the clause number (§5.35, §6.52…) unchanged.

---

*Acknowledgement: all game data comes from [18xx.games](https://18xx.games/) and the players who published their games there for free.*
