# padfeed schema

Graduations on two non-pump.fun Solana launchpads: `dbc` = Meteora Dynamic Bonding Curve (Bags, Jupiter Studio, ...) -> DAMM v2 pool, `ray` = Raydium LaunchLab (bonk.fun etc.) -> Raydium CPMM pool. Each coin followed 20 min (dbc) / 30 min (ray). Source: Solana RPC; US$ quote prices from Jupiter's free price API.

| type | field | type | meaning |
|---|---|---|---|
| all | t | number | ms when recorded |
| grad | pad | "dbc"/"ray" | launchpad |
| grad | platform | string | platform config / name |
| grad | mint, quote | string | coin mint; quote mint (SOL, or a token such as USD1/a tokenized stock) |
| grad | pool, curveAuth, baseVault, quoteVault | string/null | pool and vault accounts |
| grad | b, q | number | vault balances (tokens, quote units) at migration |
| grad | px | number | price in quote = q / b |
| grad | supply, decimals, tokenProgram | number, number, string | mint facts (Token or Token-2022) |
| grad | feeBps, freeze, mintAuth | | Token-2022 transfer fee, freeze / mint authorities (null = none) |
| grad | sig, bt / other | | migration signature, block time and when heard |
| px | pad, mint, b, q | | vault balances when they changed (every ~6 s, ~2 s for fast-moving coins); price = q / b |
| holders | pad, mint, top10, top20[] | | +60 s and +5 min: 20 largest token accounts (`acct`, `owner`, `amt`); `top10` = share held by top 10 excluding pool vaults and curve |
| qpx | usd | object | every 30 s: US$ price of SOL and of every non-SOL quote mint seen in the last 2 h, keyed by mint |
