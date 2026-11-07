# CageIQ public pick ledger

Every prediction [CageIQ](https://cageiq.app) makes, logged **before** the fight and never edited.
This repository is updated automatically after each scheduled run (Monday and Thursday). It holds one file:
[`picks.csv`](picks.csv).

## Columns

| column | meaning |
|---|---|
| `created_at` | when the pick was logged (UTC) |
| `mu_id` | matchup id (stable per fight) |
| `event`, `event_date` | the card and its date |
| `fighter_a`, `fighter_b` | the two fighters |
| `p_a` | our model's probability that `fighter_a` wins |
| `model` | model version that made the pick (`blend-v1`, `blend-v2`, …) |
| `market_p_a`, `market_src` | the betting market's de-vigged probability for `fighter_a` at that moment, and its source |

A fight can have several rows: a new row is appended only when our probability moves by more than 1 point
(for example after a model update). **The official pick is the last row logged before 00:00 UTC on the event
date.** Results are graded at [cageiq.app/results](https://cageiq.app/results).

## How to verify we never edit history

1. **Git history** of this repository: every update only appends lines to `picks.csv`.
2. **Independent snapshots** at the Internet Archive: <https://web.archive.org/web/*/cageiq.app/ledger/picks.csv>.
   Compare any snapshot with today's file: every line in the old snapshot must still be here, unchanged.
3. The SHA-256 of the current file is shown at [cageiq.app/record](https://cageiq.app/record).
