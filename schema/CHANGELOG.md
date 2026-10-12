# Schema changelog

The `schemaVersion` in every manifest points here. A version bumps when a field is added, renamed or removed in any dataset; a note that only explains existing data does not bump it.

## 1 (files since 2026-10-10)

The shape described in `gradtrades.md`, `pregrad.md`, `padfeed.md`, `meteora-pools.md`.

Known gaps in version 1, written down rather than hidden:

- `pregrad` `ctrade`: coins on curves priced in another token (not SOL) carry `sol` 0, `capAfter` null, `curveSol` 0 in files up to 2026-10-12 17:47 UTC (the SOL amount really is zero; the trade was in the other token). From then on the recorder follows SOL curves only, so later files have none of these rows. Treat `capAfter` null as "not a SOL curve". Also from then on, SOL coins near graduation are covered far better (non-SOL curves had been taking most of the recorder's follow slots).
- `gradtrades` `trade`: a `sig` of 64 `1`s is the RPC's placeholder for "signature not reported" (a few rows per million).
- `padfeed` `grad`: `pool` is null on dbc rows; `blockTime` is in ms where `bt` elsewhere is in seconds.
- Row order: `gradtrades` and `meteora-pools` are in `t` order; `pregrad` and `padfeed` are grouped by source stream, each stream in `t` order.
