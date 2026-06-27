# NSE Momentum Strategy Tracker

A lightweight, single-page dashboard for tracking dividend cycle momentum trades on the Nairobi Securities Exchange.

---

## What It Does

- Shows a live strategy table of NSE stocks with buy zones, targets, and status badges
- Automatically labels each stock: **BUY ACCUMULATION**, **MOMENTUM RUN**, **EXIT SIGNAL**, or **WATCH** based on the current date
- Fetches live prices from Yahoo Finance (falls back to simulated prices if unavailable)
- Includes a **Swap Calculator** to work out how many shares of one stock you can buy after selling another, with a 4.2% round-trip friction fee deducted

---

## How to Deploy (GitHub Pages)

1. Create a new GitHub repository
2. Upload `index.html` to the root of the repo
3. Go to **Settings → Pages → Source → main branch / root**
4. Your tracker will be live at `https://yourusername.github.io/repo-name/`

> Live prices only work when served from a web server. They will not load if you open the file directly from your computer (`file:///...`).

---

## How to Add a New Stock

Open `index.html` and find the `STOCKS` array near the top of the `<script>` tag.

Add a new object following this pattern:

```javascript
{
  name:           "Safaricom",
  ticker:         "SCOM",
  yahooTicker:    "SCOM.NR",       // NSE tickers on Yahoo use .NR suffix
  floorLow:       17.50,
  floorHigh:      19.00,
  target:         24.00,
  announcement:   "Mid-May",
  agm:            "Late June",
  buyMonths:      [3, 4],          // 0=Jan, 1=Feb ... 11=Dec
  momentumMonths: [4, 5],
  exitMonths:     [5, 6],
},
```

Then add a fallback price in the `MOCK_PRICES` object just below:

```javascript
SCOM: { price: 18.20, prev: 17.90 },
```

The new stock will appear everywhere automatically — table, ticker tape, calculator, and stats.

---

## Status Badge Logic

| Badge | Meaning | Trigger |
|---|---|---|
| 🟢 BUY ACCUMULATION | Accumulate in this window | Current month is in `buyMonths` |
| 🟡 MOMENTUM RUN | Hold — price moving to target | Current month is in `momentumMonths` |
| 🔴 EXIT SIGNAL | Start selling into strength | Current month is in `exitMonths` |
| ⚫ WATCH | Outside the active cycle | None of the above |

---

## Files

```
index.html   — the entire app (HTML + CSS + JavaScript, no build step needed)
README.md    — this file
```

---

## Tech Stack

- HTML5
- Tailwind CSS (loaded via CDN)
- Vanilla JavaScript
- Yahoo Finance public chart API (no API key required)

---

*Not financial advice. Do your own research before making any trades.*
