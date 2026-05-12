# Ettorino4Finance

A personal AI investment advisor dashboard. Single HTML file — no server, no dependencies, no build step.

## What it does

Ettorino is your personal AI financial consultant. On first launch you choose how to start:

- **Build my portfolio** — Ettorino collects your budget, goal and preferred sectors, then runs a 4-step algorithm (screening → catalysts → ranking → allocation) to propose a real portfolio from a curated universe of 120 EU+USA stocks. Uses Claude web search to verify current market conditions before proposing.
- **I already have stocks** — enter your holdings manually (ticker, name, quantity, average price) and Ettorino starts monitoring immediately.
- **Start with an empty portfolio** — jump straight to the dashboard, see AI suggestions right away, add holdings whenever you want.

Once in the dashboard:

- **Control bar** — two sliders driving the suggestion algorithm:
  - *Risk*: Conservativo / Moderato / Aggressivo
  - *Geo*: USA ←→ EU bias
- **My portfolio** — tile grid: live price, daily change, P&L, AI signal (▲ BUY / ◆ HOLD / ▼ AVOID). Click any tile for a detail popup with inline AI analysis.
- **Ettorino's picks** — AI-generated buy suggestions from 120 real EU+USA stocks, filtered by interest tags. 4-step algorithm with web search: screening → catalyst verification → ranking → output.
- **Crypto gems** — promising non-top-20 crypto from 45 curated assets + up to 2 extras per refresh.
- **Chat bubble** — floating 💬 button bottom-right opens a chat modal with Ettorino. Full portfolio context on every message. Badge lights up when Ettorino replies with the modal closed.

All AI sections refresh **manually only** — click ↻ when you want an update.

---

## Setup

### 1. Anthropic API key (required)

1. Download `ettorino4finance_standalone.html`
2. Open in **Chrome** or **Edge**
3. Get a key from [console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys)
4. Create a user, paste the key into **Anthropic API key**, click **Entra →**

### 2. Finnhub API key (optional — real-time prices)

Without it, prices are simulated. With it, all holdings update with live market data.

1. Sign up free at [finnhub.io](https://finnhub.io) — no credit card
2. Copy your key from [finnhub.io/dashboard](https://finnhub.io/dashboard)
3. At login, paste it into **Chiave API Finnhub**, click **Entra →**

The key is saved to localStorage and auto-loaded on every subsequent open. You only enter it once.

See **[FINNHUB_SETUP.md](./FINNHUB_SETUP.md)** for ticker formats and troubleshooting.

### Auto-login

If you have a single user with a saved Anthropic key, the login screen is skipped automatically on page load.

---

## Finnhub — real-time prices

| | With key | Without key |
|---|---|---|
| Prices | Live market data | Simulated random walk |
| Badge | `● live` | `● simulato` |
| Rate limit | 60 calls/min free | n/a |
| Cost | Free forever | Free |

**First time:** enter the key at login → saved. Every reload after that: auto-loaded from localStorage.

**Ticker formats for European stocks:**

| Exchange | Format | Example |
|---|---|---|
| Milan | `TICKER.MI` | `ISP.MI`, `ENEL.MI` |
| Frankfurt | `TICKER.DE` | `SAP.DE` |
| Paris | `TICKER.PA` | `AIR.PA` |
| Amsterdam | `TICKER.AS` | `ASML.AS` |
| Madrid | `TICKER.MC` | `IBE.MC` |
| London | `TICKER.L` | `RIO.L` |

Crypto (`BTC`, `ETH`, `SOL`) resolve automatically to Binance USDT pairs.

---

## Multi-user

Same file, separate users. Each user has their own Anthropic key, Finnhub key, portfolio and data — isolated in localStorage.

---

## Persistence and backup

| Mechanism | When |
|---|---|
| localStorage | On every change |
| File auto-save | Every hour — on first launch Ettorino prompts you to pick a folder |
| JSON export | Manual, `↓ JSON` |
| Markdown export | Manual, `↓ MD` |
| JSON import | At login — restores portfolio on a new machine |

---

## Price refresh

Configurable: 10s / 30s / 1m / 5m / off. With Finnhub: fetches live quotes for all holdings sequentially (200ms between calls). Without: simulated oscillation.

---

## API cost

**All AI sections are manual-refresh only.**

| Call | Model | Cost |
|---|---|---|
| AI picks (↻) | Haiku 4.5 | ~$0.004 |
| Crypto gems (↻) | Haiku 4.5 | ~$0.003 |
| AI signals (↻) | Sonnet 4 | ~$0.002 |
| Chat message | Sonnet 4 | ~$0.002 |
| Popup analysis | Sonnet 4 | ~$0.001 |
| Portfolio build (once) | Sonnet 4 + search | ~$0.015 |

**Example:** refresh picks + crypto once a day + 5 chat messages ≈ **$0.02/day → ~$0.60/month**.

Haiku is used for structured JSON tasks (picks, crypto). Sonnet for conversational quality (chat, signals, onboarding).

### Rate limits (Tier 1)

| Model | Input tokens/min |
|---|---|
| Sonnet 4 | 30,000 |
| Haiku 4.5 | 50,000 |

On 429 errors: wait 60 seconds and retry. To raise limits, add credit at [console.anthropic.com](https://console.anthropic.com).

---

## Reset

**reset** button wipes all data for the current user and relaunches onboarding.

---

## Tech stack

- Zero server, zero build — open `.html` in Chrome or Edge
- **Claude Sonnet 4** for chat, signals, onboarding
- **Claude Haiku 4.5** for picks and crypto (cost-optimised)
- **Claude web_search** (`web_search_20250305`) for real-time market data
- **Finnhub API** for live prices (optional, free)
- **Stock universe**: 120 real EU+USA tickers, 9 sectors, hardcoded
- **Crypto universe**: 45 curated non-top-20 + up to 2 extras per refresh
- **Fonts**: Inter · Bricolage Grotesque · IBM Plex Mono
- **Storage**: localStorage → sessionStorage → in-memory fallback
- **Auto-save**: File System Access API (Chrome/Edge)

---

## Files

| File | Description |
|---|---|
| `ettorino4finance_standalone.html` | The entire application |
| `README.md` | This file |
| `FINNHUB_SETUP.md` | Finnhub key setup, ticker formats, troubleshooting |