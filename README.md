<div align="center">

<img width="180" alt="MPL Indonesia" src="https://github.com/user-attachments/assets/80962392-bf6d-4781-936f-b2bafc6569fc" />

# MPL Indonesia Datasets

**Nine seasons of MPL Indonesia (S10–S18) in a single SQLite file.**
680 matches · 1,611 games · 32,219 picks/bans · 320 players · 135 heroes.

<p>
  <img src="https://img.shields.io/badge/SQLite-3.37%2B-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="sqlite" />
  <img src="https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="python" />
  <img src="https://img.shields.io/badge/STRICT%20tables-enabled-0f9d58?style=for-the-badge" alt="strict" />
  <img src="https://img.shields.io/badge/data-CC--BY--SA%203.0-EF9421?style=for-the-badge&logo=creativecommons&logoColor=white" alt="license" />
</p>

<p>
  <img src="https://img.shields.io/badge/seasons-S10%20–%20S18-black?style=flat-square" alt="seasons" />
  <img src="https://img.shields.io/badge/tables-16-black?style=flat-square" alt="tables" />
  <img src="https://img.shields.io/badge/views-9-black?style=flat-square" alt="views" />
  <img src="https://img.shields.io/badge/size-4.8%20MB-black?style=flat-square" alt="size" />
</p>

</div>

---

<div align="center">

<img src="https://github.com/user-attachments/assets/efe312c0-f818-477d-91de-657d60f21f1f" height="52" alt="ONIC" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/cc8cbe64-03d5-44a2-9cc4-8f4aa1078fde" height="52" alt="RRQ Hoshi" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/0c815049-4112-46e5-b344-d6477a1ee0b3" height="52" alt="Bigetron by VIT" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/8ffaa3cc-5588-4659-a48e-c080147ba6fb" height="52" alt="EVOS" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/0aefb7d0-171b-4a82-b14c-46147ccb621d" height="52" alt="Alter Ego" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/c3e3710c-0d7e-4b7d-9091-1de1d5a27829" height="52" alt="Team Liquid ID" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/71e0f162-e5e3-4b81-9809-0b3a9b9f7f8f" height="52" alt="Dewa United" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/ff44c260-b605-42c2-b820-b8e887309edd" height="52" alt="NAVI" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/4420131c-471e-4ab8-8ed6-43e7c6d5b0fd" height="52" alt="Aura Fire" />&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/c9434794-acff-4a03-a38f-ba6d138c8021" height="52" alt="Rebellion" />&nbsp;&nbsp;

<sub>ONIC · RRQ Hoshi · Bigetron by VIT · EVOS · Alter Ego Esports · Team Liquid ID · Dewa United · NAVI · AURA Fire · Geek Fam ID · Rebellion Esports</sub>

</div>

## What this is

A relational database rebuilt from public Liquipedia and id-mpl.com data — not a pile of
CSVs dumped into tables. Every table runs in `STRICT` mode, every relationship has a
foreign key, and `build.py` refuses to report success until the row count in the database
matches the source CSV exactly.

`mpl.db` is an **artifact, not a source**. Do not edit it directly — change `schema.sql`
and rebuild, so that no change ends up living only inside a binary file with no trace of
where it came from.

> Table, column, and view names are kept in Indonesian because they are the actual
> identifiers inside the database. A glossary is near the bottom of this page.

---

## By the numbers

| | Rows | | Rows |
|---|---:|---|---:|
| `pick_ban` | 32,219 | `laga` (matches) | 680 |
| `hero_relasi` | 6,292 | `pemain` (players) | 320 |
| `game` | 1,611 | `hero` | 135 |
| `roster` | 809 | `tim_musim` | 79 |
| `hero_musim` | 779 | `tim` (teams) | 11 |
| `award` | 611 | `musim` (seasons) | 9 |

---

## Quick start

```bash
git clone https://github.com/agaollineed/MPLDatasets.git
cd MPLDatasets

sqlite3 database/mpl.db              # open it
python database/build.py --periksa   # verify integrity
```

```python
import sqlite3, pandas as pd

con = sqlite3.connect("database/mpl.db")
pd.read_sql("SELECT * FROM v_klasemen WHERE season = 17", con)
```

No server, no installation. SQLite ships inside Python and R, and opens in DB Browser
for SQLite or DBeaver.

> **Note:** the source CSVs are not committed to this repository, so a full rebuild
> (`python database/build.py` without `--periksa`) will not run from a fresh clone.
> The shipped `mpl.db` is ready to query as is.

---

## Sample output

**Most contested hero per season** (`queries/01_evolusi_meta_hero.sql`). Presence — the
share of games where a hero was picked **or** banned — is a more honest signal than pick
count: a hero strong enough to dominate rarely gets picked, because it gets removed first.

| Season | Hero | Picks | Bans | Presence | Win rate |
|---:|---|---:|---:|---:|---:|
| S10 | Wanwan | 46 | 126 | 100.0% | 60.87 |
| S11 | Joy | 11 | 163 | 100.0% | 45.45 |
| S14 | Chip | 7 | 205 | 100.0% | 42.86 |
| S17 | Freya | 38 | 167 | 100.0% | 60.53 |

**Team dominance across seasons** (`queries/03_dominasi_tim.sql`):

| Team | Seasons | W–L | Win rate | Playoff berths | Titles |
|---|---:|---|---:|---|---:|
| ONIC | 9 | 98–35 | 73.7% | 8 / 8 | 6 |
| Bigetron by VIT | 9 | 81–51 | 61.4% | 8 / 8 | 1 |
| Team Liquid ID | 5 | 43–30 | 58.9% | 3 / 4 | 1 |
| RRQ Hoshi | 9 | 71–61 | 53.8% | 6 / 8 | 0 |

Four more example queries live in [`database/queries/`](database/queries).

---

## Repository layout

```
.
├── README.md
├── .gitignore
└── database/
    ├── mpl.db        # build artifact (4.8 MB)
    ├── schema.sql    # DDL: 16 tables, keys, CHECKs, indexes
    ├── views.sql     # 9 analytical views
    ├── build.py      # builder + verifier
    ├── queries/      # 6 example research queries
    └── README.md     # full documentation
```

📘 **[Full documentation is in `database/README.md`](database/README.md)** — per-table data
dictionary, entity relationship map, `NULL` conventions, the two symmetry rules in
`hero_relasi`, and the complete list of limitations. This page deliberately does not
repeat it, so there is only one source of truth to keep in sync.

---

## Known gaps

Read this before running any analysis:

- **No per-game results.** All 32,219 `pick_ban` rows are unlabelled for win/loss, so
  "which drafts actually win" cannot be answered from raw rows — only through the
  aggregates in `hero_musim` and `hero_relasi`. The data exists in the source HTML and is
  waiting to be scraped. This is the single biggest gap in the dataset.
- **No player ↔ hero link.** `pick_ban` records picks belonging to a *team*, not a
  *player*, so per-player hero statistics are out of reach.
- **Column coverage is uneven across seasons.** Player KDA exists only for S18; pick/ban
  data only for S10–S17. Run `queries/06_cakupan_data.sql` before anything else.
- **Seasons 1–9 are not included yet.**

---

## Glossary

| Identifier | Meaning |
|---|---|
| `laga` / `game` | match (best-of series) / individual game within a match |
| `pemain` / `tim` / `musim` | player / team / season |
| `roster` | player–team–season membership, including substitutes and staff |
| `pick_ban` | one pick or ban, tied to a specific game |
| `hero_relasi` | hero pair statistics — `rekan` (allied) or `lawan` (opposing) |
| `tim_musim` / `pemain_musim` | per-season team / player statistics |
| `lolos_playoff` | playoff qualification: `1` yes, `0` no, `NULL` not played yet |
| `v_klasemen` / `v_h2h` / `v_hero_meta` | standings / head-to-head / hero meta views |

---

## License and attribution

| Component | License |
|---|---|
| Data (`database/mpl.db`) | [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/), following [Liquipedia](https://liquipedia.net/mobilelegends/) |
| Code (`build.py`, `schema.sql`, `views.sql`, `queries/`) | MIT |
| Team logos | Trademarks of their respective organisations, used for identification |

Seasons S10–S17 come from Liquipedia; the ongoing season from
[id-mpl.com](https://id-mpl.com). **Derivative works must carry the same attribution** —
that is a share-alike requirement, not a suggestion.

This repository is not affiliated with Moonton, MPL Indonesia, or any of the teams listed.
