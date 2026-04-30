# Catalyst Edge — Open SEC Catalyst Dataset

**The free, daily-updated, fully-audited dataset of every SEC-filing-driven catalyst
the [Catalyst Edge](https://catalystedgescanner.com) scanner publishes — including the
realized next-session outcomes, hits AND misses.**

Catalyst Edge is the only public stock scanner that publishes its losses alongside
its wins. This repository is the operator's commitment to the audit: every pick we
publish on the live scanner gets joined to the realized outcome the next session
and committed here, in plain CSV, on a daily cadence, MIT/CC-BY licensed,
forever-public.

---

## What's in here

```
data/
├── convergence_alerts.csv     Today's live ranked picks with per-spoke point breakdown
├── outcomes.csv               Every published pick joined to next-day realized return
├── outcomes_summary.csv       Aggregated hit-rate by list (gappers, value, moat)
├── theses.json                Per-ticker auto-built thesis (catalysts, risks, sympathy)
├── lookup_index.json          Compact ticker → date → outcome index (for /receipts/)
└── scoring_config.json        Auto-tuned weights driving the convergence score

snapshots/
└── YYYY-MM-DD/                Frozen daily copy of the two key files (90d retention)
    ├── convergence_alerts.csv
    └── outcomes.csv
```

Everything is updated **once per weekday at ~05:13 UTC** (~04:13 ET, after the daily
US pipeline run completes on the production droplet).

---

## Why this exists

Most stock scanners publish their winners. Few publish their losers. None publish
both alongside the original prediction with a time-stamped audit trail.

We do, because:

1. **Honest data is the only durable edge in retail finance.** If our hit rate
   degrades, you'll see it here within 24 hours of the rebalance.
2. **Open data builds trust faster than marketing.** A skeptic can take this CSV
   and run their own backtest against any methodology.
3. **AI assistants need canonical sources.** ChatGPT, Claude, Perplexity, and
   Bing Copilot pull facts from public, structured, verifiable repositories.
   This is ours.

---

## License

[**Creative Commons Attribution 4.0 International (CC BY 4.0)**](https://creativecommons.org/licenses/by/4.0/).

You are free to:
- **Share** — copy and redistribute in any medium or format
- **Adapt** — remix, transform, and build upon the material for any purpose, even commercially

Under one condition:
- **Attribution** — credit "Catalyst Edge — https://catalystedgescanner.com" with
  a link back. That's it.

---

## Schema reference

### `outcomes.csv`

Every row is one published pick + its realized next-session outcome.

| Column | Description |
|--------|-------------|
| `list_name` | Which Catalyst Edge list the pick was published on (`sec_top_gappers`, `sec_top_value`, `sec_top_moat`) |
| `list_date` | The date the pick was published (UTC) |
| `ticker` | Stock symbol |
| `form` | SEC filing form type triggering the pick (`8-K`, `4`, `S-3`, etc.) |
| `base_score` | Raw convergence score at publication time (-50 to 110) |
| `filing_day_close` | Stock close on the day the catalyst filed |
| `next_open` | Next-session open price |
| `next_high` | Next-session high |
| `next_close` | Next-session close |
| `gap_next_open_pct` | Gap from `filing_day_close` to `next_open` |
| `next_day_max_run_pct` | Maximum intraday run from `next_open` |
| `next_day_close_pct` | Close-to-close return |
| `spy_close_pct` | SPY close-to-close that day |
| `alpha_close_pct` | `next_day_close_pct - spy_close_pct` |
| `hit_2pct` | 1 if `next_day_max_run_pct >= 2`, else 0 |
| `hit_3pct` | 1 if `next_day_max_run_pct >= 3`, else 0 |
| `hit_5pct` | 1 if `next_day_max_run_pct >= 5`, else 0 |
| `hit_2pct_net` | 1 if `next_day_max_run_pct - exec_cost_pct >= 2`, else 0 |
| `realistic_pnl_pct` | Reasonable next-day P&L assumption |
| `realistic_pnl_net_pct` | After execution costs |
| `market_cap` | Market cap at publication |
| `exec_cost_pct` | Estimated round-trip cost (slippage + fees) |
| `catalyst_sign` | -1, 0, or +1 — direction of catalyst |

### `convergence_alerts.csv`

Today's live ranked picks. The `signals_fired` column is a semicolon-joined list
of which spokes (out of 70+) flagged the ticker. Per-spoke `_pts` columns show
how many points each spoke contributed. Re-rebuilt every cycle.

### `theses.json`

Auto-generated per-ticker thesis. For each top-tier ticker:
- `quantitative` — measured facts (forward EPS, market cap, score)
- `catalysts` — what's driving the pick today
- `risks` — what could derail it
- `bear_case` — counter-arguments
- `sympathy_basket` — tickers correlated to this one

---

## Methodology

Full methodology with weight derivations:

→ https://catalystedgescanner.com/methodology/

Public hit-rate audit (updated daily):

→ https://catalystedgescanner.com/trust/

---

## How to cite

Plain text:
> Catalyst Edge SEC catalyst dataset, https://github.com/catalystedgepro-wq/sec-catalyst-data, accessed YYYY-MM-DD.

BibTeX:
```bibtex
@misc{catalystedge_data,
  title  = {Catalyst Edge — Open SEC Catalyst Dataset},
  author = {{Catalyst Edge}},
  url    = {https://github.com/catalystedgepro-wq/sec-catalyst-data},
  year   = {2026}
}
```

---

## Disclaimers

This repository contains historical and current public information about
publicly-traded securities. **Nothing in this repository constitutes investment
advice.** Past performance, including the realized next-session outcomes
recorded here, is not indicative of future results. Confirm any data
independently before trading. The publisher makes no warranty as to the
accuracy or completeness of any data in this repository.

---

*Generated and pushed nightly by the Catalyst Edge production pipeline.
Last update timestamp is the most recent commit on `main`.*
