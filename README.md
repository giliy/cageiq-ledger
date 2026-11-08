# CageIQ public pick ledger

Every prediction [CageIQ](https://cageiq.app) makes, logged **before** the fight and never edited.
This repository is updated automatically after each scheduled run (Monday and Thursday). It holds two files:
[`picks.csv`](picks.csv) (UFC) and [`boxing-picks.csv`](boxing-picks.csv) (boxing, since November 2026).

## Columns: `picks.csv` (UFC)

| column | meaning |
|---|---|
| `created_at` | when the pick was logged (UTC) |
| `mu_id` | matchup id (stable per fight) |
| `event`, `event_date` | the card and its date |
| `fighter_a`, `fighter_b` | the two fighters |
| `p_a` | our model's probability that `fighter_a` wins |
| `model` | model version that made the pick (`blend-v1`, `blend-v2`, …) |
| `market_p_a`, `market_src` | the betting market's de-vigged probability for `fighter_a` at that moment, and its source |

## Columns: `boxing-picks.csv` (boxing)

| column | meaning |
|---|---|
| `created_at` | when the pick was logged (UTC) |
| `event_id` | the fight's id in our odds feed (stable per fight) |
| `fight_date` | the scheduled start (UTC) |
| `boxer_a`, `boxer_b` | the two boxers |
| `p_a` | CageIQ's probability that `boxer_a` wins. For boxing this **starts from the betting market** and adjusts it with both boxers' records (`box-blend-v1`) |
| `model` | model version |
| `market_p_a`, `market_src` | the market's de-vigged probability for `boxer_a` at that moment (Pinnacle, or the median of the books) |

In both files a fight can have several rows: a new row is appended only when our probability moves by more than
1 point. **The official pick is the last row logged before 00:00 UTC on the fight's date.** UFC results are graded at
[cageiq.app/results](https://cageiq.app/results), boxing at [cageiq.app/boxing](https://cageiq.app/boxing/#record).

## How to verify we never edit history

1. **Git history** of this repository: every update only appends lines to `picks.csv` and `boxing-picks.csv`.
2. **Independent snapshots** at the Internet Archive: <https://web.archive.org/web/*/cageiq.app/ledger/picks.csv> and
   <https://web.archive.org/web/*/cageiq.app/ledger/boxing-picks.csv>. Compare any snapshot with today's file: every
   line in the old snapshot must still be here, unchanged.
3. The SHA-256 of each current file is shown at [cageiq.app/record](https://cageiq.app/record) (UFC) and
   [cageiq.app/boxing](https://cageiq.app/boxing/#verify) (boxing).
