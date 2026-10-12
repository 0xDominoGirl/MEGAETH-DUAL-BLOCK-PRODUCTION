# MegaETH Dual Block Production

A browser-based visualizer of MegaETH mini blocks and EVM blocks. It shows a live
transaction stream, block statistics, and links to transactions in Blockscout.
The interface, styling, and JavaScript are contained in `index.html`.

## Run locally

Install Git and Python 3, then run:

```sh
git clone https://github.com/0xDominoGirl/MEGAETH-DUAL-BLOCK-PRODUCTION.git
cd MEGAETH-DUAL-BLOCK-PRODUCTION
python3 -m http.server 8000 --bind 127.0.0.1
```

Open <http://127.0.0.1:8000>. Stop the server with `Ctrl+C`.
There is no package installation or build step. A modern browser and an internet
connection are required for the live data sources and Google Fonts.
The current configuration uses public endpoints and does not ask for a wallet
connection or an API key.

## Using the visualizer

- Watch the mini-block and EVM-block columns update while the pointer is outside
  the block grid.
- Hover over the grid to pause updates and inspect a transaction or block.
- Use transaction tooltips to open the corresponding Blockscout page.
- Expand an EVM block's additional transactions with its overflow control.
- Move the pointer away from the grid and tooltip to resume the display.

## Data sources and configuration

The configuration constants near the beginning of the script control the
endpoints and display limits:

| Setting | Current value or purpose |
| --- | --- |
| `WS_URL` | `wss://mainnet.megaeth.com/rpc`; subscribes to `miniBlocks` |
| `HTTP_RPC` | `https://mainnet.megaeth.com/rpc`; HTTP JSON-RPC requests |
| `EXPLORER` | `https://megaeth.blockscout.com/tx/`; transaction links |
| `MAX_MINI` / `MAX_EVM` | Retains up to 500 mini blocks / 100 EVM blocks |
| `EVM_TX_CAP` | Initially displays up to 16 transactions per EVM block |

The transaction-count card reads `transactions_today` from
`https://megaeth.blockscout.com/api/v2/stats` every 60 seconds and can fall back
to a cached value in browser local storage. Although the card is labelled
"Transaction Count (24h)", it uses the upstream `transactions_today` field;
do not assume it is an independently calculated rolling 24-hour count.

`CONTRACT_NAMES` maps known contract addresses to display labels. Endpoint
availability, rate limits, and supported subscription methods affect live
updates. A replacement WebSocket endpoint must support `miniBlocks`.

## Troubleshooting

- **Red connection indicator:** check browser network access to `WS_URL`. The
  page attempts to reconnect three seconds after a WebSocket connection closes.
- **Connected but no blocks:** inspect the browser console and WebSocket frames
  for subscription errors or missing notifications.
- **Frozen display:** move the pointer outside the grid and tooltip; hovering
  deliberately pauses updates.
- **Stale transaction count:** inspect the Blockscout stats request. A cached
  value can remain visible when that request fails.
- **RPC errors:** inspect the browser's Network panel for failed requests,
  rate limits, or cross-origin restrictions before changing the endpoints.
