# PriceSylo

PriceSylo watches cryptocurrency markets on your own computer and tells you what is
happening in them. It connects straight to the exchanges, analyses the price action and
the order flow as it arrives, and keeps that picture ready for you, your own programs,
and AI assistants.

Everything runs on this machine. There is no account, no cloud service and no telemetry:
the only things that leave your computer are the exchange connections themselves, any
webhooks you set up, and a licence check once every four hours.

---

## Contents

1. [Install it](#1-install-it)
2. [Start it for the first time](#2-start-it-for-the-first-time)
3. [Everyday commands](#3-everyday-commands)
4. [Watch your first market](#4-watch-your-first-market)
5. [Where your files live](#5-where-your-files-live)
6. [Settings you might want to change](#6-settings-you-might-want-to-change)
7. [Your licence](#7-your-licence)
8. [If something goes wrong](#8-if-something-goes-wrong)
9. [For developers](#9-for-developers)
10. [Support](#10-support)

---

## 1. Install it

PriceSylo is **one file**. It carries everything it needs inside it, so you do not need to
install Node.js, a package manager, or anything else — on any operating system.

| Your system | The file you run |
|---|---|
| macOS (Intel and Apple Silicon) | `PriceSylo-universal` |
| Windows | `PriceSylo-win-x64.exe` |
| Linux | `PriceSylo-linux-x64` |

Put it wherever you like and open it. The first time you run it, it installs itself
properly: it copies itself to a permanent home, registers itself to start with your
computer, and adds a `pricesylo` command you can type in any terminal. It never
overwrites anything that is already there, so installing a newer version keeps your
licence, your tracked markets and your saved data.

On Linux, make it executable first:

```
chmod +x PriceSylo-linux-x64
./PriceSylo-linux-x64
```

### Your computer will warn you the first time

These builds are not code-signed yet, so your operating system will say it does not
recognise the publisher. This is a signature check, not a virus warning, and it happens
once per download.

- **macOS** — the first double-click is refused. Open **System Settings → Privacy &
  Security**, scroll to the bottom, and click **Open Anyway** next to the PriceSylo line.
- **Windows** — SmartScreen shows a blue box. Click **More info**, then **Run anyway**.
- **Linux** — no prompt; just make the file executable as above.

### Only one engine per computer

PriceSylo is a trading engine, so two copies running at once would be a fault rather than
an inconvenience. If one is already running, a second one tells you so and stops. This is
also why the copy in your `Downloads` folder asks you to use the `pricesylo` command
instead of running itself a second time.

---

## 2. Start it for the first time

Run the file, or type `pricesylo` in a terminal. It asks you one question:

```
This engine will not start without a licence.

  1) Use a license key
  2) Test the product as a demo
     (each run lasts 30 minutes, then the engine stops and clears its market data)

Choice [1/2]:
```

**If you bought a licence**, choose 1 and paste the key from your confirmation email. It
looks like `LX-XXXXX-XXXXX-XXXXX-XXXXX`. It is saved, so you are only asked once.

**If you are trying PriceSylo out**, choose 2. The demo is the same program with two
limits, and it is a good way to see whether it suits you:

- **It runs for 30 minutes**, then stops on its own. Start it again as often as you like —
  there is no daily limit and no cooldown.
- **Every run starts fresh.** The analysis it builds up is cleared when it starts, so a
  demo never accumulates the depth a licensed engine develops by running continuously.
  Your settings file is never touched.

Neither limit can be switched off by a setting, and you can move from the demo to a
licence at any time with `pricesylo license activate <YOUR-KEY>`.

Once it starts you will see something like this:

```
INFO  [app] engine ready - favorites=0 streams=0 keeper=13645
INFO  [app] rest api on http://127.0.0.1:8420/api
```

That address is the engine, on your machine only.

---

## 3. Everyday commands

You will mostly use the first three.

| Command | What it does |
|---|---|
| `pricesylo` | Starts it if it is not running, then shows you what it is doing, live |
| `pricesylo status` | A health check: is it installed, running, licensed, and answering? |
| `pricesylo logs --follow` | Watch what the engine is printing |
| `pricesylo stop` / `start` / `restart` | Stop it, start it, or both — and wait until it has actually happened |
| `pricesylo license status` | What your licence allows and when it expires |
| `pricesylo license deactivate` | Release this computer's slot so you can use it on another |
| `pricesylo uninstall` | Remove it. Add `--purge` to delete your settings and saved data too |
| `pricesylo --help` | Everything else |

Press `Ctrl+C` while watching to stop *watching* — the engine keeps running.

---

## 4. Watch your first market

PriceSylo only analyses markets you ask it to. Adding one is called **favouriting** it,
and everything else follows from that: once a pair is a favourite, its live analysis,
order flow and alerts are all available.

If you use the PriceSylo desktop app, add pairs from there and skip this section.

Otherwise, three commands — find a pair, track it, read it:

```bash
# 1. find the exact symbol
curl -s "http://127.0.0.1:8420/api/spot/pairs?platform=binance&query=BTCUSDT&limit=3"

# 2. start tracking it on two timeframes
curl -s -X POST http://127.0.0.1:8420/api/spot/favorites \
  -H 'Content-Type: application/json' \
  -d '{"platform":"binance","symbol":"BTCUSDT","timeframes":["1m","5m"]}'

# 3. read who is in control right now
curl -s "http://127.0.0.1:8420/api/spot/context/binance/BTCUSDT/orderflow/dominancy"
```

```json
{"ok":true,"data":{"dominant_side":"BUYERS","buy_pressure":94.77,"sell_pressure":5.22,"strength":89.55}}
```

Give a new favourite about fifteen seconds of live data before expecting order-flow
answers — prices can be read straight away, but order flow has to be *watched* as it
happens, and cannot be fetched from history.

Your favourites are remembered and resume automatically the next time the engine starts.

---

## 5. Where your files live

Everything PriceSylo keeps is in its install folder. `pricesylo status` prints the path.

| File | What it is |
|---|---|
| `config.json` | Your settings. Safe to edit — see the next section |
| `.engine-state/` | Saved analysis, favourites and licence record. Deleting it loses your tracked markets |
| `logs/` | What the engine printed, rolled over when it gets large |
| `error.log` | Crashes, if there ever are any |

Back up `config.json` if you have put exchange keys in it.

---

## 6. Settings you might want to change

Open `config.json` in any text editor. Most changes take effect immediately, without a
restart — the engine re-reads the file when you save it.

| Setting | What it does |
|---|---|
| `server.port` | The port the engine listens on. Change it if `8420` is taken |
| `server.host` | Keep it `127.0.0.1` unless you know you want it reachable from other machines |
| `logging.level` | `debug`, `info`, `warn` or `error`. `info` is the default |
| `credentials` | Your exchange API keys, if you want account features. Read-only keys are enough for analysis |
| `proxy` | Set this if your network only reaches the internet through a proxy |

If an edit is invalid the engine keeps running on the previous settings and prints a line
naming exactly what was wrong — it never half-applies a broken file.

---

## 7. Your licence

PriceSylo is commercial software from **Laxtic Software Services**. The `LICENSE` file
beside this one is the agreement.

|  | Personal | Commercial |
|---|---|---|
| Term | 1 year, renewable | 2 years, renewable |
| Computers | up to 3 | up to 5 |
| Tracked markets | 5 | unlimited |
| Timeframes per market | 3 | all 5 |

Everything described in this guide works on both. They differ in the limits above, not in
what you can see.

**How the check works.** The engine verifies your licence when it starts and every four
hours after that. It sends your key, a random installation id created on this machine, your
computer's name and the version — and nothing else. No market data, no keys, no analysis,
no usage of any kind. Your exchange connections never pass through it.

**If the licence service is unreachable, the engine keeps working for seven days** on its
last confirmed answer. Only a definite refusal stops it, never an outage or a flight with
no wifi.

**Moving to another computer.** Run `pricesylo license deactivate` on the old one to free
the slot. If the machine is gone, support can release it for you.

If you are behind a company firewall, the engine needs outbound HTTPS to the licence
service. It refuses rather than quietly bypassing a proxy it cannot use.

---

## 8. If something goes wrong

| What you see | What it means | What to do |
|---|---|---|
| It exits straight away with a licence error | The key is expired, revoked, or all your computer slots are in use | The message says which. `pricesylo license status`, or contact support |
| It retries for two minutes, then exits | It could not reach the licence service | Check this machine can reach the internet over HTTPS. Set `proxy` in `config.json` if you go through one |
| It stopped on its own after running fine for a while | A licence re-check was definitively refused — usually an expired term | Renew, then start it again |
| It stops after exactly 30 minutes | You are running the demo | Activate a licence: `pricesylo license activate <KEY>` |
| `address already in use ... 8420` | Something already has that port — often a second copy | `pricesylo stop`, or change `server.port` |
| "no context for … pair not in favorites" | You have not asked it to track that market yet | Add it as a favourite (section 4) |
| A market you just added says "degraded" | Its history could not be fetched yet | It recovers on live data. Check your connection to that exchange |
| Nothing at all for a few seconds after a restart | Saved analysis is waiting for the first live update | Wait a moment and try again |
| Your OS says the publisher is unknown | The build is not code-signed yet | See section 1 — allow it once |

`pricesylo status` is the fastest way to see which part is unhappy. If you contact
support, send its output.

---

## 9. For developers

The engine exposes the same data three ways, all on `127.0.0.1:8420` and all local:

| Surface | Address | Good for |
|---|---|---|
| REST | `http://127.0.0.1:8420/api` | Scripts, dashboards, anything that can make an HTTP request |
| WebSocket | `ws://127.0.0.1:8420/stream` | Live updates pushed to you as they happen |
| MCP | `http://127.0.0.1:8420/mcp` | AI assistants — the same capabilities as tools |

Every REST reply has the same shape: `{"ok":true,"data":…}` or `{"ok":false,"error":…}`.
Both the WebSocket and MCP surfaces mirror the REST routes exactly, so anything you can
read one way you can read the others.

You can also set up **webhooks**: rules that watch a value and POST to a URL of yours when
it crosses a line you set. They are signed so your receiver can verify them.

`GET /api/health` tells you it is alive and what phase it is in; `GET /api/status` adds
what it is currently tracking. The complete reference — every route, every channel, every
tool, the webhook signature scheme and the exchange methods — is available from support.

---

## 10. Support

**support@pricesylo.com**

Please include the output of `pricesylo status` and, if it is relevant, the last few lines
of `pricesylo logs`. Your exchange keys never appear in either, and your licence key is
shown only in masked form.

PriceSylo is published by **Laxtic Software Services**. Your use of it is governed by the
`LICENSE` file that ships beside this one.
