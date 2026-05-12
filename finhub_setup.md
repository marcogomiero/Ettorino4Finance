# Getting Your Finnhub API Key

Finnhub provides real-time stock prices for free, with a limit of 60 API calls per minute — more than enough for Ettorino4Finance.

## Step-by-step

**1. Create a free account**

Go to [finnhub.io](https://finnhub.io) and click **Get free API key** or **Sign up**.

Fill in your name, email and a password. No credit card required.

**2. Confirm your email**

Check your inbox for a confirmation email from Finnhub and click the verification link.

**3. Find your API key**

After logging in, go to your [Dashboard](https://finnhub.io/dashboard). Your API key is shown at the top of the page under **API Key**.

It looks like this: `ct1abc2def3ghi4jkl5mno6`

**4. Enter it in Ettorino4Finance**

In the Ettorino4Finance login screen, paste your Finnhub key into the **Chiave API Finnhub** field (below the Anthropic key field).

The key is saved only in your browser's localStorage and is never sent anywhere other than Finnhub's API.

---

## What Finnhub is used for

| Data | Endpoint | Free tier |
|---|---|---|
| Real-time quote (price, change %) | `/quote` | ✓ |
| Company profile (name, sector) | `/stock/profile2` | ✓ |
| Basic financials | `/stock/metric` | ✓ |

Ettorino fetches a fresh quote for each holding on every price refresh cycle (configurable: 10s / 30s / 1m / 5m). With 60 calls/minute available, you can comfortably track up to 50 positions on a 30-second refresh.

---

## Coverage

Finnhub covers all major exchanges including:

- **USA**: NYSE, Nasdaq, AMEX
- **Europe**: Euronext (Paris, Amsterdam, Brussels, Lisbon), Frankfurt (XETRA), Milan (Borsa Italiana), Madrid, Stockholm, Helsinki, Oslo
- **Crypto**: real-time prices via Binance, Coinbase and other exchanges

For European tickers, use the exchange suffix format:

| Exchange | Format | Example |
|---|---|---|
| Milan (Borsa Italiana) | `TICKER.MI` | `ISP.MI`, `FBK.MI` |
| Frankfurt (XETRA) | `TICKER.DE` | `SAP.DE`, `BMW.DE` |
| Paris (Euronext) | `TICKER.PA` | `LVMH.PA`, `AIR.PA` |
| Amsterdam | `TICKER.AS` | `ASML.AS`, `PHIA.AS` |
| Madrid | `TICKER.MC` | `IBE.MC`, `SAN.MC` |
| Stockholm | `TICKER.ST` | `VOLV-B.ST` |
| London | `TICKER.L` | `GLEN.L`, `RIO.L` |

For crypto, use `BINANCE:BTCUSDT`, `BINANCE:ETHUSDT` format — or just enter `BTC` and Ettorino will resolve it automatically.

---

## Limits and notes

- **Free tier**: 60 calls/minute, unlimited daily calls
- **No credit card** required for the free plan
- The free plan is permanent — it does not expire
- If you exceed 60 calls/minute, the API returns a 429 error and Ettorino will retry on the next refresh cycle
- For websocket real-time streaming (sub-second updates), a paid plan is required — not needed for Ettorino

---

## Troubleshooting

**"Invalid API key"** — double-check you copied the full key from the Finnhub dashboard with no extra spaces.

**Price shows "—"** — the ticker format may be wrong. Try adding the exchange suffix (e.g. `ENEL.MI` instead of `ENEL`).

**European stocks not updating** — Finnhub free tier covers European markets but with a 15-minute delay for some exchanges during market hours. US stocks are real-time.

**Crypto not updating** — use the `BINANCE:XYZUSDT` format or just the symbol (e.g. `BTC`, `ETH`) and Ettorino resolves it via the Finnhub crypto endpoint.