# fastmint — free-mint racer for the Binance Agentic Wallet (baw)

First-come-first-served free-mint race toolkit for BNB Chain, built for the
Binance Agentic Wallet (`baw`).

`fastmint` claims free agent-ID mints at the open second. It prepares everything
before the window — calldata, wallet session, RPC warm-up — so the only step
left at open is the execute.

## How it works

- **fire.js** — the shooter. Prechecks the wallet session, balance and lock,
  warms up six public BSC RPCs, spins `eth_call` across them every 120 ms from
  2 seconds before open, then fires the Binance wallet preview and executes
  exactly once. Flags: `--dry`, `--eager`, `--once`, `--start <unix or HH:MM[:SS] WIB>`.
- **recon.js** — contract recon: reads the mint function, free pool counter,
  phase, price and whether the wallet already claimed, so a race is only fired
  when the mint is actually free and open.
- **lib.js** — shared helpers: RPC list, calldata builders, receipt parsing.

## Race setup

- Two parallel layers, so a failed preview on one never blocks the execute.
- Six public BSC RPCs raced together; never pipe a long-running mint script
  through `head` (SIGPIPE kills it).
- Gas reserve is deliberately tiny (0.00001 BNB) so it never blocks a free mint.
- Lowest gas price for every mint.

## Results

Built for the free-mint races of `jackdefi.ai`: FEG, SIDECOIN, BUCKET,
The Claf Eye, Agent Island and more — all claimed free, gas only.

## Requirements

- Node.js 22+
- Binance Agentic Wallet (`baw`) with an active session
- An Agent ID (this toolkit was built for `jackdefi.ai`)

Free mints only. No paid mints, no paid shares, no private keys.
