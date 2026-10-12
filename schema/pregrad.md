# pregrad schema

Who is behind each pump.fun launch, as raw facts (addresses, amounts, times). No wallet labels, no risk flags, no images. Sources: PumpPortal free websocket (creations, migrations), Solana public RPC (bonding curves, transactions). Amounts in SOL; `pct` = % of the 1,000,000,000 supply; times in ms unless the name ends in `S` (seconds).

| type | field | type | meaning |
|---|---|---|---|
| create | t, mint, sig | number, string, string | coin creation |
| create | creator | string | creator wallet |
| create | name, sym | string | coin name / ticker |
| create | devTokens, devSol | number | creator's own buy at creation |
| create | capSol | number | curve market cap right after creation |
| create | curve | string | bonding-curve account |
| create | mayhem | bool | pump.fun "mayhem mode" launch |
| curvex | slot, curve, mint | number, string, string/null | bonding-curve cap crossing (mint null when not yet mapped) |
| curvex | cap, cross, sol, sinceFirstS | number | cap now (SOL), line crossed (100/200/300/400 SOL), SOL in curve, seconds since first seen |
| ctrade | bt, slot, sig, i, mint, side, user | | bonding-curve trade on a coin near graduation (read from cap 200 SOL up) |
| ctrade | sol, tokens, capAfter, curveSol | number | SOL and tokens of the trade, cap and curve SOL after it |
| migrate | mint, sig, pool | string | migration seen by PumpPortal (`pool` = destination AMM) |
| grad | mint, slot, capSol, creator | | graduation as seen on chain |
| record | mint, sym, name, pool, slot0, sig0, t0, bt0 | | one per graduate, written 10-40 s after graduation (t0 = graduation) |
| record | src | object | which feed saw it first; `curveCompleteS` = curve completed how long before t0 |
| record | creator, create | string, object | creator wallet; creation tx (`t`, `sig`, `mayhem`, `capSol`) |
| record | ageS | number | creation to graduation, seconds |
| record | instant | bool | graduated within 5 s of creation |
| record | cap0, curveSol | number | PumpSwap cap at graduation; SOL raised |
| record | devBuyTokens, devBuyPct, devBuySol | number | creator's buy at creation |
| record | creatorPctAtGrad | number/null | creator's holding at graduation |
| record | creatorInfo | object | `txs50` (tx count, capped 50), `capped`, `oldestS`, `nBefore10m`, `funder`, `fundSol`, `fundedBeforeS`, `fundMemo`, `priorCoins24h`, `priorCoins72h` |
| record | gradSlot | object/null | PumpSwap buys in the graduation slot: `pct`, `sol`, `nWallets`, `nTx`, `buys[]` (`w`, `tokens`, `pct`, `sol`, `sig`, `bundleSize`, `payer`, `signers`, `slotOffset`, `creatorInTx`), `whale` = the largest of those buys with its wallet facts (`txs25`, `oldestS`, `lastTxKind`, `funder`, `fundSol`, `fundedBeforeS`, `fundSig`, `fundMemo`, `gainers[]`) |
| record | curve | object | `coverage` (full/partial/none), `holders[]` (top 10: `w`, `pct`, `sol`, `creator`), `creationSlot`, `bundleEst`, `estimated`, `top1NcPct`, `top5NcPct`, `nOver3Pct`, `bundleWallets`, `bundlePct`, `creatorPct`, `path` (`t100S`..`t400S`, `maxCap`, `updates`, `completeS`), `trades` |
| outcome | mint, sym, t0, recT | | +10 min after graduation |
| outcome | capAt | object | PumpSwap cap (SOL) at +10/45/60/120/180/300/600 s |
| outcome | maxGain, minDrop | number | best / worst move in %, first 10 min |
| outcome | crashed | bool | fell by 80%+ |
| outcome | trades | number | PumpSwap trades counted |
| outcome | dump, firstBigSell, bigSells[] | object | the first sale breaking a fixed size rule and the large sales: `tS`, `w`, `frac` (share of pool sold), `sol`, `capAfter`, `rule`, `tokensFrom` |
| outcome | src | string | where trades were read from |

Removed before sale: our internal "rogue" wallet list and every field derived from it, `flags`, `priorRugged`, the dumper `role`, picture fingerprints and metadata URIs, heartbeat/stats rows.
