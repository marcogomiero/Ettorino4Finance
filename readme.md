# Ettorino4Finance

A personal AI investment advisor dashboard. Single HTML file — no server, no dependencies, no build step.

## What it does

Ettorino is your personal AI financial consultant. On first launch you choose how to start:

- **Build my portfolio** — Ettorino collects your budget, goal and preferred sectors, then runs a 4-step algorithm (screening → catalysts → ranking → allocation) to propose a real portfolio from a curated universe of 120 EU+USA stocks. Uses Claude web search to verify current market conditions before proposing.
- **I already have stocks** — enter your holdings manually (ticker, name, quantity, average price) and Ettorino starts monitoring them immediately.
- **Start with an empty portfolio** — jump straight into the dashboard, see AI suggestions right away, add holdings whenever you want.

Once in the dashboard:

- **Control bar** — two sliders that drive the entire suggestion algorithm:
  - *Risk*: Conservativo / Moderato / Aggressivo — changes candidate universe, allocation weights and constraints passed to Claude
  - *Geo*: USA ←→ EU — shifts the geographic bias of suggestions from 100% USA to 100% EU
- **My portfolio** (top section) — tile grid: live price, daily change, P&L and AI signal (▲ BUY / ◆ HOLD / ▼ AVOID). Click any tile to open a detail popup with inline AI analysis. Double-click to edit.
- **Ettorino's picks** (middle section) — AI-generated buy suggestions from a fixed universe of 120 real EU+USA stocks. 4-step algorithm: screening → catalyst verification → ranking by conviction → output. Interest tags let you filter by sector. Anti-repetition memory across refreshes.
- **Crypto gems** (bottom section) — promising non-top-20 crypto from a curated list of 45 assets plus up to 2 Claude-discovered extras per refresh.
- **News** — real-time news via Claude web search, scoped to your tickers, translated into Italian.
- **Chat** — contextual advisor with your full portfolio passed on every message.
- **Detail popup** — click any tile to get a short inline AI analysis and send to chat.

All AI sections refresh **manually only** — click ↻ on each section when you want an update. This keeps costs predictable.

---

## Setup

### Required: Anthropic API key

1. Download `ettorino4finance_standalone.html`
2. Open it in **Chrome** or **Edge**
3. Go to [console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys) and create a key
4. Create a user in Ettorino and paste the key into the **Anthropic API key** field

### Optional but recommended: Finnhub API key (real-time prices)

Without a Finnhub key, prices are simulated. With it, every holding updates with live market data.

1. Go to [finnhub.io](https://finnhub.io) — click **Get free API key**, no credit card required
2. Find your key at [finnhub.io/dashboard](https://finnhub.io/dashboard)
3. Paste it into the **Finnhub API key** field at login

See **[FINNHUB_SETUP.md](./FINNHUB_SETUP.md)** for ticker formats, coverage and troubleshooting.

---

## Finnhub integration

| Feature | With key | Without key |
|---|---|---|
| Portfolio prices | Real-time market data | Simulated random walk |
| Live badge | `● live` with timestamp | `● simulato` |
| Rate limit | 60 calls/min free — handles ~50 holdings on 30s refresh | n/a |

Finnhub is always free — zero cost to add.

---

## Multi-user

Same file, separate users. Each user has their own Anthropic key, Finnhub key, portfolio and data — isolated in localStorage.

---

## Persistence and backup

| Mechanism | When |
|---|---|
| localStorage | On every change |
| File auto-save | Every hour — on first launch Ettorino prompts you to pick a folder |
| JSON export | Manual, `↓ JSON` button |
| Markdown export | Manual, `↓ MD` button |
| JSON import | At login — to restore on a new machine |

---

## Price refresh

Configurable: 10s / 30s (default) / 1m / 5m / off. With Finnhub enabled each cycle fetches live quotes. Without Finnhub, prices oscillate randomly.

---

## API cost

**All AI sections are manual-refresh only** — you pay only when you click ↻.

### Models used

| Call type | Model | Why |
|---|---|---|
| News, picks, crypto gems | Claude Haiku 4.5 | ~20× cheaper, sufficient for structured JSON output |
| Chat, popup analysis, signals | Claude Sonnet 4 | Higher quality for conversational and analytical responses |
| Portfolio construction (onboarding) | Claude Sonnet 4 + web search | One-time, quality matters |

### Cost per manual refresh (approx.)

| Section | Cost |
|---|---|
| News (↻) | ~$0.003 |
| AI picks (↻) | ~$0.004 |
| Crypto gems (↻) | ~$0.003 |
| AI signals (↻) | ~$0.002 |
| Chat message | ~$0.002 |
| Popup analysis | ~$0.001 |
| Onboarding (once) | ~$0.015 |

**Example:** refreshing all three sections once a day + 5 chat messages ≈ **$0.02/day → ~$0.60/month**.

### Anthropic rate limits (Tier 1)

| Model | Input tokens/min | Output tokens/min |
|---|---|---|
| Claude Sonnet 4 | 30,000 | 8,000 |
| Claude Haiku 4.5 | 50,000 | 10,000 |

If you hit a 429 error, wait 60 seconds and retry — the limit resets per minute. To raise limits, add credit at [console.anthropic.com](https://console.anthropic.com) — Tier 2 requires $50 spent lifetime.

---

## Reset

The **reset** button wipes all data for the current user and relaunches onboarding.

---

## Tech stack

- Zero server, zero build — open the `.html` file in Chrome or Edge
- **Claude Sonnet 4** (`claude-sonnet-4-20250514`) for chat, signals and onboarding
- **Claude Haiku 4.5** (`claude-haiku-4-5-20251001`) for news, picks and crypto (cost-optimised)
- **Claude web_search tool** (`web_search_20250305`) for real-time data
- **Finnhub API** for live stock and crypto prices (optional, free)
- **Stock universe**: 120 real EU+USA tickers across 9 sectors (hardcoded, no hallucination)
- **Crypto universe**: 45 curated non-top-20 projects + up to 2 extras per refresh
- **Fonts**: Inter · Bricolage Grotesque · IBM Plex Mono
- **Storage**: localStorage → sessionStorage → in-memory fallback
- **Auto-save**: File System Access API (Chrome/Edge only)

---

## Files

| File | Description |
|---|---|
| `ettorino4finance_standalone.html` | The entire application |
| `README.md` | This file |
| `FINNHUB_SETUP.md` | Guide to getting and configuring your Finnhub key |