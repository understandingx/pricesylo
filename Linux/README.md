# PriceSylo

PriceSylo watches cryptocurrency markets on your own computer and tells you what is
happening in them. It connects straight to the exchanges, analyses the price action and
the order flow as it arrives, and keeps that picture ready for you, your own programs,
and AI assistants.

Everything runs on this machine. There is no account, no cloud service and no telemetry:
the only things that leave your computer are the exchange connections themselves, any
webhooks you set up, and the licence check (at start and every four hours).

---

## Contents

1. [Which file you need](#1-which-file-you-need)
2. [Start it for the first time](#2-start-it-for-the-first-time)
3. [Commands](#3-commands)
4. [Watch your first market](#4-watch-your-first-market)
5. [Where your files live](#5-where-your-files-live)
6. [Settings you might want to change](#6-settings-you-might-want-to-change)
7. [Your licence](#7-your-licence)
8. [If something goes wrong](#8-if-something-goes-wrong)
9. [For developers](#9-for-developers)
10. [Support](#10-support)

---

## 1. Which file you need

| Your system | What you run |
|---|---|
| **macOS, Windows** | Nothing separate: the engine is **inside the PriceSylo app**. Install the app and open it; it starts the engine, and stops it when you quit. Enter your licence key in the app (Help > Engine). Most of this manual is about the Linux engine; sections 4, 6, 7 and 9 apply to every system. |
| **Linux** | `PriceSylo-linux-x64`, a single file with everything it needs inside it: no Node.js, no package manager, nothing else to install. The desktop app for Linux is a separate download and connects to this engine. |

On Linux, keep the engine together with `config.json`, `LICENSE` and this `README.md` in
one folder you can write to (for example `~/PriceSylo`). The engine keeps its settings and
everything it saves **in that folder**, so a read-only location will not work. It does not
install itself anywhere else, and it does not start by itself when you log in (see
section 3 if you want that).

```
chmod +x PriceSylo-linux-x64
./PriceSylo-linux-x64
```

### Only one engine per computer

PriceSylo is a trading engine, so two copies running at once would be a fault rather than
an inconvenience: two feeds, alerts firing twice, two sets of orders on one exchange
account. Before it does anything, a new engine checks whether one is already running on
this computer - from any folder, including the one inside the desktop app - and if so it
says where that one is and exits (without touching it). It also refuses to start when
another program holds its port, and names the setting that moves it.

---

## 2. Start it for the first time

Run the file in a terminal. It asks you one question:

```
This engine will not start without a licence.

  1) Use a license key
  2) Test the product as a demo
     (each run lasts 30 minutes, then the engine stops and clears its market data)

Choice [1/2]:
```

**If you bought a licence**, choose 1 and paste the key from your confirmation email. It
looks like `LX-XXXXX-XXXXX-XXXXX-XXXXX`. It is saved, so you are only asked once.

**If you are trying PriceSylo out**, choose 2. The demo is the same program with fixed
limits (section 7), and it is a good way to see whether it suits you:

- **It runs for 30 minutes**, then stops on its own. Start it again as often as you like -
  there is no daily limit and no cooldown.
- **Every run starts fresh.** The analysis it builds up is cleared when it starts, so a
  demo never accumulates the depth a licensed engine develops by running continuously.
  Your settings file is never touched.

The engine remembers the demo choice and starts as the demo each time after that. Neither
limit can be switched off by a setting. To move to a licence at any time, stop the engine
and run `./PriceSylo-linux-x64 license activate <YOUR-KEY>`; the next start is licensed.

**Without a terminal** (a service, a server), there is no question to answer: give the key
with `license activate <KEY>` first, or put it in `config.json` (`license.key`), or set
`PRICESYLO_LICENSE_KEY`, or pass `--license-key <KEY>`. `--demo` starts the demo instead.

Once it starts you will see something like this:

```
INFO  [app] engine ready - favorites=0 streams=0 keeper=13645
INFO  [app] rest api on http://127.0.0.1:8420/api
```

The first start can take a minute or more while market data warms up. That is normal.

---

## 3. Commands

| Command | What it does |
|---|---|
| `./PriceSylo-linux-x64` | Starts the engine in this terminal. It keeps running until you stop it |
| `./PriceSylo-linux-x64 --demo` | Starts it as the 30-minute demo, without asking |
| `Ctrl+C` (or `kill <pid>`) | Stops it in order: connections closed, state saved |
| `./PriceSylo-linux-x64 license status` | What your licence allows, and whether this engine may run |
| `./PriceSylo-linux-x64 license info` | The details: plan, device id, expiry, seats |
| `./PriceSylo-linux-x64 license activate <KEY>` | Save a licence key for this install |
| `./PriceSylo-linux-x64 license change <KEY>` | Replace the saved key with another |
| `./PriceSylo-linux-x64 license deactivate --yes` | Release this computer's seat so you can use the licence on another |

The engine runs in the terminal that started it; closing that terminal stops it. To keep it
running in the background and start it with your computer, run it as a service once a key is
saved (`license activate`). For example, as a systemd user service in
`~/.config/systemd/user/pricesylo.service`:

```
[Unit]
Description=PriceSylo engine

[Service]
ExecStart=%h/PriceSylo/PriceSylo-linux-x64
WorkingDirectory=%h/PriceSylo
Restart=on-failure

[Install]
WantedBy=default.target
```

then `systemctl --user enable --now pricesylo`. Only one engine can run at a time (section 1),
so stop the terminal copy first.

---

## 4. Watch your first market

PriceSylo only analyses markets you ask it to. Adding one is called **favouriting** it,
and everything else follows from that: once a pair is a favourite, its live analysis,
order flow and alerts are all available.

If you use the PriceSylo desktop app, add pairs from there and skip this section.

Otherwise, three commands find a pair, track it, read it:

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
answers: prices can be read straight away, but order flow has to be *watched* as it
happens, and cannot be fetched from history.

Your favourites are remembered and resume automatically the next time the engine starts.

---

## 5. Where your files live

**Linux:** everything is in the folder that holds the engine. **macOS and Windows:** in
`~/Library/Application Support/PriceSylo` (macOS) or `%LOCALAPPDATA%\PriceSylo` (Windows),
managed by the app.

| File | What it is |
|---|---|
| `config.json` | Your settings. Safe to edit, see the next section |
| `.engine-state/` | Saved analysis, favourites and the licence record. Deleting it loses your tracked markets and the saved licence |
| `engine-status.json` | What the engine is doing right now (starting, warming, ready, stopped, and why) |
| `error.log` | Crashes and licence refusals, if there ever are any |
| `logs/` | macOS and Windows only: what the engine printed (the app keeps these). On Linux the engine prints to its terminal; redirect it if you want a file |

Back up `config.json` if you have put exchange keys in it.

---

## 6. Settings you might want to change

Open `config.json` in any text editor. Most changes take effect immediately, without a
restart: the engine re-reads the file when you save it. The listen address and port are the
exception - they apply at the next start.

| Setting | What it does |
|---|---|
| `server.port` | The port the engine listens on. Change it if `8420` is taken |
| `server.host` | The address it listens on. The default, `0.0.0.0`, lets the desktop app on your other devices on the same network reach it; set `127.0.0.1` to keep it to this computer only |
| `logging.level` | `debug`, `info`, `warn` or `error`. `info` is the default |
| `credentials` | Your exchange API keys, if you want account features. Read-only keys are enough for analysis |
| `proxy` | Set this if your network only reaches the internet through a proxy |

The environment variables `HOST` and `PORT` override `server.host` and `server.port`.

If an edit is invalid the engine keeps running on the previous settings and prints a line
naming exactly what was wrong: it never half-applies a broken file.

---

## 7. Your licence

PriceSylo is commercial software from **Laxtic Software Services**. The `LICENSE` file
beside this one is the agreement.

|  | Personal | Commercial | Demo |
|---|---|---|---|
| Term | 1 year, renewable | 2 years, renewable | 30-minute runs |
| Computers | up to 3 | up to 5 | - |
| Tracked markets | 3 | unlimited | 1 |
| Timeframes per market | 3 | all 5 | 3 |

Everything described in this guide works on every plan. They differ in the limits above,
not in what you can see.

**How the check works.** The engine verifies your licence when it starts and every four
hours after that. It sends your key, a random installation id created on this machine, your
computer's name, its platform and the engine version, and nothing else. No market data, no
keys, no analysis, no usage of any kind. Your exchange connections never pass through it.

**When the licence service cannot be reached:**

- A **running** engine keeps working and retries every three minutes. If the service is
  still unreachable after an hour, the engine stops (and says so); start it again once the
  connection is back.
- A licensed install can still **start** without reaching the service for up to seven days
  after its last confirmed check.
- A **first** activation needs the service: it retries a few times over about a minute,
  then exits with a message.

Only a definite refusal (an expired, revoked or over-used key) stops it outright.

**Moving to another computer.** Run `license deactivate --yes` on the old one to free the
seat (the Linux engine's command; for the engine inside the macOS or Windows app, or if the
machine is gone, support can release it for you).

If you are behind a company firewall, the engine needs outbound HTTPS to the licence
service. It refuses rather than quietly bypassing a proxy it cannot use.

---

## 8. If something goes wrong

| What you see | What it means | What to do |
|---|---|---|
| It exits straight away with a licence error | The key is expired, revoked, or all your computer seats are in use | The message says which. `license status`, or contact support |
| It retries for about a minute, then exits | It could not reach the licence service on a first start | Check this machine can reach the internet over HTTPS. Set `proxy` in `config.json` if you go through one |
| It stopped on its own after running fine for a while | A licence re-check was refused, or the service was unreachable for an hour | The message and `error.log` say which. Renew, or restore the connection, then start it again |
| It stops after exactly 30 minutes | You are running the demo | `license activate <KEY>`, then start it again |
| "A PriceSylo engine is already running on this computer" | Another copy is running (a terminal, a service, or the desktop app's) | Use that one; the message says where it is. Stop it first if you want to run this one |
| "port 8420 is in use by another program" | Another program holds the engine's port | Stop that program, or change `server.port` |
| "no context for … pair not in favorites" | You have not asked it to track that market yet | Add it as a favourite (section 4) |
| A market you just added says "degraded" | Its history could not be fetched yet | It recovers on live data. Check your connection to that exchange |
| Nothing at all for a few seconds after a restart | Saved analysis is waiting for the first live update | Wait a moment and try again |
| Your OS says the publisher is unknown | The build is not code-signed yet | Allow it once (on Linux there is no prompt) |

`license status`, `engine-status.json` and the last lines the engine printed are the fastest
way to see which part is unhappy. If you contact support, send them.

---

## 9. For developers

The engine exposes the same data three ways, on port `8420` of the machine it runs on:

| Surface | Address | Good for |
|---|---|---|
| REST | `http://127.0.0.1:8420/api` | Scripts, dashboards, anything that can make an HTTP request |
| WebSocket | `ws://127.0.0.1:8420/stream` | Live updates pushed to you as they happen |
| MCP | `http://127.0.0.1:8420/mcp` | AI assistants: the same capabilities as tools |

Every REST reply has the same shape: `{"ok":true,"data":…}` or `{"ok":false,"error":…}`.
Both the WebSocket and MCP surfaces mirror the REST routes exactly, so anything you can
read one way you can read the others.

You can also set up **webhooks**: rules that watch a value and POST to a URL of yours when
it crosses a line you set. They are signed so your receiver can verify them.

`GET /api/health` tells you it is alive and what phase it is in; `GET /api/status` adds
what it is currently tracking. The complete reference (every route, every channel, every
tool, the webhook signature scheme and the exchange methods) is available from support.

---

## 10. Support

**support@pricesylo.com**

Please include the output of `license status`, your `engine-status.json` and, if it is
relevant, the last lines the engine printed (or `error.log`). Your exchange keys never
appear in any of them, and your licence key is shown only in masked form.

PriceSylo is published by **Laxtic Software Services**. Your use of it is governed by the
`LICENSE` file that ships beside this one.
