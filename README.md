# CageIQ public pick ledger

Every prediction [CageIQ](https://cageiq.app) makes, logged **before** the fight and never edited.
This repository is updated automatically after every scheduled run (several times a week). The picks:
[`picks.csv`](picks.csv) (UFC), [`boxing-picks.csv`](boxing-picks.csv) (boxing, since November 2026) and
[`promotions-picks.csv`](promotions-picks.csv) (PFL, KSW, RIZIN). Three frozen public experiments, each graded at
cageiq.app: [`edge-watch.csv`](edge-watch.csv), [`price-watch.csv`](price-watch.csv) and [`poly-watch.csv`](poly-watch.csv).

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

## `price-watch.csv` (since 14 November 2026)

Price watch flags the betting favourite when CageIQ's chance for it is at least 5 points above the market's, the
first time that's true before picks lock, and records the price at that moment. After the lock the flag is graded
against the closing line at [cageiq.app/price-watch](https://cageiq.app/price-watch/). One row per fight, never edited.

| column | meaning |
|---|---|
| `flagged_at` | when the flag was logged (UTC) |
| `mu_id`, `event`, `event_date` | the fight |
| `fighter`, `opponent` | the flagged favourite and its opponent |
| `judge_p`, `market_p` | CageIQ's and the market's (de-vigged) chance for `fighter` at the flag |
| `line_p` | the median sportsbook's de-vigged chance in the same odds snapshot (compared with the close) |
| `price`, `best_price`, `best_book`, `books` | middle and best decimal price for `fighter` in that snapshot, and the number of books |

## `poly-watch.csv` (since 15 November 2026)

Polymarket gap watch: on every run, each open UFC fight that has both a sportsbook price and a Polymarket price gets
one row with both, taken minutes apart. The official observation of a fight is its last row before 00:00 UTC on the
fight's date; a fight is flagged when Polymarket and the sportsbooks differ by 3 points or more there. A second rule
(also from 15 November 2026) counts only flags where at least $10,000 had been traded on Polymarket. Results are
summarised on [cageiq.app/research/ufc-polymarket-accuracy](https://cageiq.app/research/ufc-polymarket-accuracy/).
Rows are never edited.

| column | meaning |
|---|---|
| `seen_at` | when both prices were read (UTC) |
| `mu_id`, `event`, `event_date`, `fighter_a`, `fighter_b` | the fight |
| `poly_p_a` | Polymarket's mid price for `fighter_a` (only when the bid/ask spread is 10 cents or less) |
| `book_p_a`, `book_src` | the sportsbooks' de-vigged chance for `fighter_a` (Pinnacle, else the median book) |
| `poly_volume` | the Polymarket market's traded volume in dollars at that moment |

## `eye-notes.csv` (since 15 November 2026)

CageIQ's eye test: notes from watching a fighter's fight, written after it, from a fixed list of tags (faded late,
durability concern, looked injured, looked past their best, weight-cut trouble, strong late, hurt but recovered,
looked improved). Each note is shown before the fighter's next fight and is never part of the AI's number. The public
test: for each noted fighter's next UFC fight, did they win more or less often than the closing odds expected?
Nothing is reported as a finding before 100 graded fights in a group. A withdrawn note is a new row (`retracts`).

| column | meaning |
|---|---|
| `logged_at` | when the note was logged (UTC), always after the fight it describes |
| `fighter`, `opponent`, `fight_slug`, `event`, `fight_date` | the fight that was watched |
| `tags` | tags from the fixed list, `;`-separated (`retracted` on a withdrawal row) |
| `note` | the observation in plain words |
| `retracts` | on a withdrawal row: the `logged_at` of the note it withdraws |

## How to verify we never edit history

1. **Git history** of this repository: every update only appends lines to `picks.csv` and `boxing-picks.csv`.
2. **Independent snapshots** at the Internet Archive: <https://web.archive.org/web/*/cageiq.app/ledger/picks.csv> and
   <https://web.archive.org/web/*/cageiq.app/ledger/boxing-picks.csv>. Compare any snapshot with today's file: every
   line in the old snapshot must still be here, unchanged.
3. The SHA-256 of each current file is shown at [cageiq.app/record](https://cageiq.app/record) (UFC) and
   [cageiq.app/boxing](https://cageiq.app/boxing/#verify) (boxing).

## Research datasets (CC BY 4.0)

CageIQ also publishes its UFC research with every table as a CSV, refreshed after each card, with sample sizes, 95%
intervals and what the closing betting odds expected on the same fights:
[cageiq.app/research](https://cageiq.app/research/). Examples: how often UFC favourites win, whether age, reach or
southpaw stance matter, home advantage, referees and judges, and Polymarket against the sportsbooks. Each study page
has a "Download the tables (CSV)" link. Free to reuse with a link to the study.

## How to cite

> CageIQ public pick ledger, https://github.com/giliy/cageiq-ledger (accessed <date>). Live record:
> https://cageiq.app/record/

Questions or corrections: https://cageiq.app/contact/
