# Solana Launch Tape

Clean, daily data files about what happens when new Solana meme coins launch and graduate: every trade, who is behind each launch, other launchpads, and Meteora pool metrics. Recorded 24/7 by our own recorders since early October 2026.

**Free sample:** a 2-hour cut of every dataset (2026-10-10 14:00-16:00 UTC) is in `sample/`. **Buy:** see Pricing at the bottom.

## The datasets

| Dataset | What it is | Rows a day (Oct 10 2026) | Since |
|---|---|---|---|
| `gradtrades` | Every buy and sell on PumpSwap in the first 30 minutes after each pump.fun graduation, with wallet, amounts, price, pool reserves and fees | ~5,034,000 | Oct 3 2026 |
| `pregrad` | Who is behind each pump.fun launch: creator, its funding, curve buyers, graduation-slot buyers, the cap path; plus a +10 min price outcome. Raw facts only | ~194,000 | Oct 4 2026 |
| `padfeed` | Graduations on Meteora DBC (Bags, Jupiter Studio...) and Raydium LaunchLab (bonk.fun): migration facts, pool balances every few seconds, top holders, quote prices in US$ | ~32,500 | Oct 3 2026 |
| `meteora-pools` | Per-minute price, TVL, volume and exact per-minute fees for ~200 Meteora DLMM pools | ~287,000 | Oct 6 2026 |

**History is short and honest:** the oldest files are from October 3 2026, and it grows by one day every day. Coverage gaps (a dropped connection, a coin skipped under our data budget) are written into the files as rows (`down`, `up`, `resume`, `end` with `why`), never filled in or guessed.

## How it is collected

- Only public sources: Solana's public RPC, PumpPortal's free public feed, Meteora's public API, Jupiter's free price API.
- Nothing private: no keys, no logins, no scraped profiles or social handles.
- No wallet labels: we sell what happened on chain (addresses, amounts, times). We do not sell our own opinions on wallets ("insider", "rugger" and the like). Those fields are removed before export.
- Our own trading wallets are removed from every file.
- Every line is parsed and checked before it ships; malformed rows are dropped.

## Files and update cadence

- One gzipped JSON-lines file per dataset per UTC day: `gradtrades-YYYY-MM-DD.jsonl.gz`, `pregrad-...`, `padfeed-...`, `meteora-pools-...`.
- A new day is ready daily, after the UTC day closes.
- Read them with anything: `zcat file.jsonl.gz | jq`, pandas `read_json(..., lines=True)`, DuckDB `read_json_auto`.

## Schemas

Full field-by-field tables are in `schema/`:

- [`schema/gradtrades.md`](schema/gradtrades.md): one JSON object per PumpSwap trade (wallet, side, tokens, SOL, effective price, pool reserves after, lpFee/protocolFee/creatorFee), plus `grad`, `end`, `down`/`up`, `resume` rows that mark coverage.
- [`schema/pregrad.md`](schema/pregrad.md): `create`, `curvex`, `ctrade`, `migrate`, `grad` rows; one `record` per graduate (creator, its funder, dev buy, graduation-slot buyers with bundle sizes, top curve holders, cap path) and an `outcome` row at +10 min (cap at +10..600 s, max gain, min drop, first big sell).
- [`schema/padfeed.md`](schema/padfeed.md): `grad` (pad, platform, mint, quote, pool, vault balances, price, supply, Token-2022 facts), `px` (vault balances every few seconds), `holders` (top 20 at +60 s and +5 min), `qpx` (US$ quote prices every 30 s).
- [`schema/meteora-pools.md`](schema/meteora-pools.md): one row per pool per minute: price, tvlUsd, vol24hUsd, fees24hUsd, vol24hDeltaUsd, feesMinUsd (exact fees that minute).

## Licence

Personal and research use. You may not resell or republish the files, or share them outside your own team. Derived results (charts, papers, models, strategies) are yours.

No warranty. This is market data, not advice.

## Pricing

0.05 SOL per day file (one dataset, one UTC day) · 0.5 SOL per month of history (one dataset, 30 days) · 1 SOL per month for the daily feed (every dataset, each new day as it closes).

**How to buy:** send the amount in SOL to `4uooiS8jeqKuiEf81NwrE2LPEmyT54eDaefEjsuP4BEK` with a memo (your email or Telegram handle, and what you want, e.g. `feed @yourhandle` or `gradtrades 2026-10-10 you@mail.com`). Your download link follows within 24 hours. Questions: open an issue on this repo.
