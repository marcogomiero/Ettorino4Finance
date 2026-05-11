# Ettorino4Finance

A personal AI investment advisor dashboard. Single HTML file — no server, no dependencies, no build step.

## What it does

Ettorino is your personal AI financial consultant. On first launch it walks you through a short conversation to understand your budget, investment goal (growth, income, capital protection) and preferred sectors, then proposes a real portfolio with quoted tickers and amounts calculated on your exact capital. You can accept it, tweak it, or rebuild it from scratch.

Once in the dashboard:

- **Portfolio** — position list with live price, daily change and P&L per holding; allocation donut chart; invested vs residual cash summary
- **Chart** — price chart for the selected holding with position detail (quantity, average cost, P&L in €)
- **Watchlist** — cards for all your holdings with momentum indicator
- **AI Signals** — Ettorino analyses every position and returns ▲ BUY / ◆ HOLD / ▼ AVOID with a short rationale
- **News** — feed filtered by portfolio relevance ("YOURS" tag on tickers you own), one-click AI impact analysis
- **Chat** — contextual advisor with your full live portfolio passed on every message

## Setup

1. Download `ettorino4finance_standalone.html`
2. Open it in **Chrome** or **Edge** (required for file auto-save)
3. Create a user, paste your API key from [console.anthropic.com](https://console.anthropic.com/settings/keys)
4. Ettorino builds your portfolio via chat

Your API key is stored only in your browser and never sent anywhere other than Anthropic's API.

## Multi-user

Same file, separate users. Each user has their own API key, portfolio and data — fully isolated in the browser's localStorage.

## Persistence and backup

| Mechanism | When |
|---|---|
| localStorage | On every change |
| File auto-save | Every hour (pick a folder with the 📁 button) |
| JSON export | Manual, `↓ JSON` button |
| Markdown export | Manual, `↓ MD` button |
| JSON import | At login — to restore on a new machine |

> If the browser blocks localStorage (Edge with Tracking Prevention on local files), data stays in memory for the session. Use Chrome or export to JSON regularly.

## Price refresh

Configurable from the topbar: 10s / 30s (default) / 1m / 5m / off. Prices are simulated — for live data you would need to wire in a market API such as Yahoo Finance or Alpha Vantage.

## API cost

Claude Sonnet 4 — roughly $0.002–0.005 per message. Normal usage (a few analyses per day) stays well under $2/month per user.

## Reset

The **reset** button in the topbar wipes all data for the current user and relaunches the onboarding conversation with Ettorino.

## Tech stack

- Zero server, zero build — open the `.html` file in your browser
- **Claude Sonnet 4** via direct browser API (`anthropic-dangerous-direct-browser-access`)
- **Chart.js** 4.4 for charts
- **Fonts**: Inter · Bricolage Grotesque · IBM Plex Mono
- **Storage**: localStorage → sessionStorage → in-memory (fallback chain)
- **Auto-save**: File System Access API (Chrome/Edge only)