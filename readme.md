# Ettorino4Finance

A personal AI investment advisor dashboard. Single HTML file — no server, no dependencies, no build step.

## What it does

Ettorino is your personal AI financial consultant. On first launch you choose how to start:

- **Build my portfolio** — Ettorino asks your budget, goal and preferred sectors, then proposes a real portfolio with quoted tickers and amounts calculated on your exact capital. Accept it or iterate.
- **I already have stocks** — enter your holdings manually (ticker, name, quantity, average price) and Ettorino starts monitoring them immediately.
- **Start with an empty portfolio** — jump straight into the dashboard, see AI suggestions right away, add holdings whenever you want.

Once in the dashboard:

- **My portfolio** (top section) — tile grid with one card per holding: live price, daily change, P&L and AI signal (▲ BUY / ◆ HOLD / ▼ AVOID). Empty state with prompt to add positions.
- **Ettorino's picks** (bottom section) — AI-generated buy suggestions based on your profile and current market conditions, sourced via real-time web search. Refreshed every 30 minutes. Click any tile to get a full analysis in chat.
- **News** — real-time news fetched by Claude with web search, scoped to the tickers you own, translated into Italian. Auto-refresh every 30 minutes. Alert badge on urgent items.
- **Chat** — contextual advisor with your full live portfolio passed on every message. Quick-action buttons for Review, Buy/Sell, Risks, Liquidity.
- **Portfolio summary bar** — value, invested, total P&L, daily change and residual cash in a single top bar.

## Setup

1. Download `ettorino4finance_standalone.html`
2. Open it in **Chrome** or **Edge** (required for file auto-save)
3. Create a user, paste your API key from [console.anthropic.com](https://console.anthropic.com/settings/keys)
4. Choose how to start — Ettorino guides you from there

Your API key is stored only in your browser and never sent anywhere other than Anthropic's API.

## Multi-user

Same file, separate users. Each user has their own API key, portfolio and data — fully isolated in the browser's localStorage.

## Persistence and backup

| Mechanism | When |
|---|---|
| localStorage | On every change |
| File auto-save | Every hour — on first launch Ettorino prompts you to pick a folder (choose the same folder as the HTML file) |
| JSON export | Manual, `↓ JSON` button |
| Markdown export | Manual, `↓ MD` button |
| JSON import | At login — to restore on a new machine |

> If the browser blocks localStorage (Edge with Tracking Prevention on local files), data stays in memory for the session. Use Chrome or export to JSON regularly.

## Price refresh

Configurable from the topbar: 10s / 30s (default) / 1m / 5m / off. Prices are simulated — for live data you would need to wire in a market API such as Yahoo Finance or Alpha Vantage.

## API cost

Claude Sonnet 4 — roughly $0.002–0.005 per message. With AI suggestions and news refreshing every 30 minutes, expect around $3–6/month per user under normal usage. Signals and suggestions are batched to minimise calls.

## Reset

The **reset** button in the topbar wipes all data for the current user and relaunches the onboarding.

## Tech stack

- Zero server, zero build — open the `.html` file in your browser
- **Claude Sonnet 4** via direct browser API (`anthropic-dangerous-direct-browser-access`)
- **Claude web_search tool** for real-time news and market-aware suggestions
- **Fonts**: Inter · Bricolage Grotesque · IBM Plex Mono
- **Storage**: localStorage → sessionStorage → in-memory (fallback chain)
- **Auto-save**: File System Access API (Chrome/Edge only), prompted on first launch