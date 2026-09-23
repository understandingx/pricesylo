# PriceSylo

**Real-time crypto market intelligence and trading, running entirely on your own computer.**

PriceSylo connects straight to eight cryptocurrency exchanges, reads their live markets as they
move, and turns that raw feed into something you can act on: clean charts, order-flow analysis,
market context, and a trading desk for your own exchange accounts. There is no cloud service
behind it and no account to create. Your market data, your exchange keys and your notes stay on
your machine.

[**Download the latest release**](https://github.com/understandingx/pricesylo/releases/downloads-20260923-150508) &nbsp;·&nbsp; [Website](https://pricesylo.com) &nbsp;·&nbsp; [Quick start](README.txt) &nbsp;·&nbsp; [Support](mailto:support@pricesylo.com)

---

## What it does

PriceSylo is two programs that work together:

- **The engine** runs quietly on your computer. It keeps live connections to the exchanges,
  analyses price action and order flow as it arrives, and keeps that picture ready - for the
  panel, for your own scripts, and for AI assistants.
- **The panel** is the desktop app you look at. It finds the engine on your computer by itself
  and gives you the whole picture in one window.

### In the panel

- **Charts** - candlestick charts with drawing tools and volume profile, and order-flow charts
  that show where buying and selling actually happened in the order book.
- **Market context** - who is in control right now, large (whale) orders, icebergs and spoofed
  orders, and live market events, each scored by how urgent it is.
- **Trading** - place, track and cancel orders and watch your open positions on your own exchange
  accounts. Move a resting futures limit order by dragging its line on the chart.
- **Strategies and backtests** - write strategies, test them against history, and read a scored
  report of every run.
- **Trade journal** - your trades are recorded automatically, with statistics, a calendar and
  daily summaries; trades placed elsewhere can be added by hand.
- **Watchlist and pair catalogue** - pick the markets you follow across every supported exchange.
- **An AI assistant** that can read the same live market picture you see.

### Exchanges

Binance, Bybit, OKX, Bitget, KuCoin, Gate.io, Kraken and MEXC - spot and futures on every one.

### For developers and AI agents

The engine exposes the same live data on one local port, `127.0.0.1:8420`, reachable only from
your own computer:

| Surface | Address | Documentation |
|---|---|---|
| REST | `http://127.0.0.1:8420/api` | [pricesylo.com/api-documentation](https://pricesylo.com/api-documentation) |
| WebSocket | `ws://127.0.0.1:8420/stream` | [pricesylo.com/websocket](https://pricesylo.com/websocket) |
| MCP (for AI agents) | `http://127.0.0.1:8420/mcp` | [pricesylo.com/mcp](https://pricesylo.com/mcp) |

Webhooks can watch a value and notify a URL of yours when it crosses a line you set; deliveries
are signed so your receiver can verify them.

---

## Download

Everything is on the [**releases page**](https://github.com/understandingx/pricesylo/releases/downloads-20260923-150508). For your system you need the panel
installer and the engine:

| System | Panel (desktop app) | Engine |
|---|---|---|
| **macOS** - Apple Silicon and Intel | `macos-panel-PriceSylo-App-<version>-universal.dmg` | `macos-PriceSylo-universal` |
| **Windows** - x64 | `windows-panel-PriceSylo-App-<version>-win-x64-setup.exe` | `windows-PriceSylo-win-x64.exe` |
| **Linux** - x64, glibc-based distributions | `linux-panel-PriceSylo-App-<version>-linux-x86_64.AppImage`, `.deb` or `.rpm` | `linux-PriceSylo-linux-x64` |

The engine is a single file with everything it needs inside - there is nothing else to install,
not even Node.js. Next to it, keep the small files from your system's folder in this repository
([`macOs`](macOs), [`Windows`](Windows) or [`Linux`](Linux)): `config.json` (its settings),
`LICENSE` and `README.md` (the full manual).

## Getting started

1. **Start the engine.** Put it and its files in one folder you can write to, then open it.
   The first time, it asks for your licence key - or offers a 30-minute demo if you want to try
   PriceSylo first.
2. **Install and open the panel.** It finds the engine on your computer and starts drawing charts.
3. **Connect an exchange (optional).** Charts and analysis work without any account. To see your
   balances and trade, add your exchange API keys in the panel's encrypted vault.

The step-by-step guide for each system, including the one-time security prompts, is in
[**README.txt**](README.txt). The full manual is `README.md` in your system's folder.

## Verify your download

Each system's folder has a `SHA256SUMS-pricesylo.txt` listing the engine and its files. On macOS
or Linux, in a folder holding those files under the names in that list, run:

```
shasum -a 256 -c SHA256SUMS-pricesylo.txt
```

Every line should say `OK`. Release downloads carry a system prefix (for example
`macos-PriceSylo-universal`); rename a file to the name in the list before checking it.

---

## Privacy

PriceSylo runs on your computer and listens only on `127.0.0.1`. There is no account and no usage
tracking. The only connection it makes to us is the licence check, which sends your licence key,
a random installation id, your computer's name and the version - nothing about your markets, your
trades or your exchange accounts.

## Licence

PriceSylo is commercial software with Personal and Commercial licences - see
[pricesylo.com](https://pricesylo.com) for what each includes. The `LICENSE` file beside the engine
is the agreement.

Orders placed through PriceSylo are real orders on your own exchange accounts. Market analysis is
information, not financial advice.

## Support

**support@pricesylo.com** &nbsp;·&nbsp; [pricesylo.com](https://pricesylo.com)

PriceSylo is published by Laxtic Software Services.
