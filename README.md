# Pulse
A Startup Metrics Dashboard


██████╗ ██╗   ██╗██╗     ███████╗███████╗
██╔══██╗██║   ██║██║     ██╔════╝██╔════╝
██████╔╝██║   ██║██║     ███████╗█████╗  
██╔═══╝ ██║   ██║██║     ╚════██║██╔══╝  
██║     ╚██████╔╝███████╗███████║███████╗
╚═╝      ╚═════╝ ╚══════╝╚══════╝╚══════╝


# 📊 Pulse — Startup Growth Intelligence Dashboard

A zero-dependency, single-file HTML dashboard for tracking startup growth metrics. Drop in your data via CSV upload, JSON upload, or manual entry — and instantly get a full picture of DAU/MAU stickiness, conversion funnels, cohort retention, churn analysis, and a north star health score.

> **No backend. No build step. No npm install.** Open the file in a browser and go.

---

## ✨ Features

### Data Ingestion
- **CSV Upload** — raw event logs (`user_id, date, event`) or pre-aggregated monthly rows
- **JSON Upload** — array of event objects or monthly metric objects
- **Manual Entry** — type in monthly figures directly with a form UI; add/edit/delete rows before building
- **Demo Dataset** — one-click 12-month simulated startup to explore the dashboard instantly

### North Star Metric
Live health panel with four automated signals:
- DAU/MAU stickiness vs. benchmarks (10% / 20% / 50%+ tiers)
- Monthly churn rate vs. danger thresholds (4% / 8%)
- Signup-to-paid conversion rate vs. SaaS average (3–5%)
- MoM MAU growth rate

Each signal renders as 🟢 healthy / 🟡 warning / 🔴 critical with a plain-English explanation.

### KPI Cards (8 metrics)
| Metric | Description |
|---|---|
| Monthly Active Users | Total MAU for latest month |
| Daily Active Users | Average DAU for latest month |
| New Signups | Acquisition volume |
| Conversions | Paid conversions |
| Revenue | Monthly revenue ($) |
| Churned Users | Users lost with MoM delta |
| Conversion Rate | Signups → Paid % |
| ARPU | Average revenue per paid user |

All cards show MoM delta with directional arrows and color-coded sentiment.

### Conversion Funnel
Six-stage funnel: **Visitors → Signups → Activated → Engaged → Converted → Retained**

- Proportional fill bars for instant visual comparison
- Percentage of total shown per stage
- Drop-off % between each stage
- Biggest drop-off stage auto-flagged in red 🔴

### Cohort Retention Heatmap
Color-coded week-over-week retention table (W0–W6) for the last 6 monthly cohorts. Green = strong retention, red = heavy drop-off. Adjusts based on your actual churn data.

### Drop-off Ranking
Horizontal bar chart ranking all funnel transitions by volume of users lost — so you know exactly where to focus.

### Time-Series Charts (5 charts via Chart.js)
| Chart | What it shows |
|---|---|
| DAU / MAU Trend | Dual-line growth over time |
| Revenue & Signups | Bar + line combo, dual Y-axis |
| Churn Rate | Monthly churned users bar chart |
| Stickiness Ratio | DAU/MAU % line with 20% benchmark reference |
| Conversion Rate | Signup-to-paid % over time |

### AI Insight Banner
Auto-generated plain-English diagnosis based on your data — flags the single most important thing to fix (churn, conversion, stickiness, or growth) with a recommended action.

---

## 🚀 Getting Started

```bash
git clone https://github.com/your-username/pulse-metrics
cd pulse-metrics
open startup_metrics_board.html   # macOS
# or just double-click the file in Finder / Explorer
```

No server required. Works fully offline after the initial Google Fonts + Chart.js CDN load.

---

## 📁 File Structure

```
pulse-metrics/
└── startup_metrics_board.html   # entire app — one file
└── README.md
└── sample_data/
    ├── sample_events.csv         # raw event log example
    ├── sample_monthly.csv        # pre-aggregated monthly example
    └── sample_monthly.json       # JSON format example
```

---

## 📄 Data Format

### Option A — Raw Event Log (CSV)

The dashboard will auto-aggregate this into monthly buckets.

```csv
user_id,date,event
u001,2024-01-03,signup
u001,2024-01-07,active
u002,2024-01-05,signup
u002,2024-01-20,purchase
u003,2024-02-01,signup
u003,2024-03-15,churn
```

**Supported event values:** `signup` · `register` · `join` · `active` · `purchase` · `convert` · `paid` · `subscribe` · `churn` · `cancel` · `unsubscribe` · `visit` · `view` · `pageview`

Optional columns: `amount` / `revenue` / `value` (for revenue per purchase event)

---

### Option B — Pre-Aggregated Monthly (CSV)

If you already have monthly rollups, use this format directly:

```csv
month,visitors,signups,dau,mau,conversions,revenue,churned
2024-01,12000,1800,420,3200,180,9200,95
2024-02,15000,2200,540,4100,230,12000,110
2024-03,19000,2900,680,5400,310,16500,130
```

| Column | Type | Description |
|---|---|---|
| `month` | `YYYY-MM` | Month identifier (required) |
| `visitors` | integer | Top-of-funnel visitors |
| `signups` | integer | New user registrations |
| `dau` | integer | Average daily active users |
| `mau` | integer | Monthly active users |
| `conversions` | integer | Paid conversions |
| `revenue` | number | Total revenue ($) |
| `churned` | integer | Users who churned |

All numeric columns default to `0` if missing or blank.

---

### Option C — JSON

```json
[
  { "month": "2024-01", "visitors": 12000, "signups": 1800, "dau": 420, "mau": 3200, "conversions": 180, "revenue": 9200, "churned": 95 },
  { "month": "2024-02", "visitors": 15000, "signups": 2200, "dau": 540, "mau": 4100, "conversions": 230, "revenue": 12000, "churned": 110 }
]
```

Also accepts a wrapper object: `{ "data": [...] }` or `{ "rows": [...] }`.

---

## 🎛️ Manual Entry

Click **Manual Entry** in the ingest panel to open the form. Fill in values for each month, click **+ Add Row** to stage it, then **Build Dashboard →** when ready. You can delete or overwrite any row before building.

---

## 📐 Metric Definitions & Benchmarks

| Metric | Formula | Benchmark |
|---|---|---|
| DAU/MAU (Stickiness) | `DAU ÷ MAU × 100` | < 10% critical · 20%+ healthy · 50%+ elite |
| Monthly Churn Rate | `Churned ÷ MAU × 100` | < 2% excellent · < 5% acceptable · > 8% critical |
| Conversion Rate | `Conversions ÷ Signups × 100` | < 2% poor · 3–5% SaaS average · > 8% strong |
| ARPU | `Revenue ÷ Conversions` | Varies by segment |
| MoM MAU Growth | `(MAU_current - MAU_prev) ÷ MAU_prev × 100` | < 5% slow · 10%+ healthy · 20%+ strong |

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---|---|---|
| [Chart.js](https://www.chartjs.org/) | 4.4.1 | All time-series and bar charts (CDN) |
| [Syne](https://fonts.google.com/specimen/Syne) | — | Display / heading font (Google Fonts CDN) |
| [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) | — | Monospace / data font (Google Fonts CDN) |

Zero npm dependencies. Zero build tools. Vanilla JS ES6+.

---

## 🔧 Customization

All colors are CSS custom properties at the top of the `<style>` block:

```css
:root {
  --accent:  #4fffb0;   /* primary green */
  --accent2: #ff4e6a;   /* red / danger */
  --accent3: #4ea8ff;   /* blue */
  --accent4: #ffc84e;   /* amber */
}
```

To change benchmark thresholds (e.g. churn danger zone), search for the relevant value in the `renderNorthStar()` function in the `<script>` block.

---

## 🤝 Contributing

Pull requests welcome. Some ideas for extension:

- [ ] Export dashboard as PDF / PNG
- [ ] `localStorage` persistence for manual data
- [ ] Week-over-week view in addition to monthly
- [ ] Integration with Mixpanel / Amplitude CSV exports
- [ ] LTV and payback period calculations
- [ ] Multi-product / segment comparison view

---

## 📝 License

MIT — use it, fork it, ship it.
