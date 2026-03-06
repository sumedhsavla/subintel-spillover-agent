# SubIntel — Substitution Intelligence Dashboard

A browser-based analytics dashboard that models out-of-stock events, maps product substitution behavior, simulates margin impact, and predicts future stockout risk with intelligent replacement recommendations.

**[Live Demo →](https://YOUR_USERNAME.github.io/subintel-dashboard/)**

![Dashboard Preview](preview.png)

---

## The Problem

Retail stockouts cost the industry over **$1 trillion annually**. But not all stockouts are equal. When Whole Milk goes out of stock, most customers grab 2% Milk — the store retains the sale. When Atlantic Salmon goes out of stock, customers often leave entirely — the revenue is lost.

Traditional inventory systems treat every stockout the same: item is low, reorder item. They don't answer the critical questions:
- **Where does the demand go** when a product goes OOS?
- **How much margin** is the store actually losing after substitution?
- **Which stockouts should be fixed first** based on financial impact?
- **Which items are about to go OOS** before it happens?

SubIntel answers all four.

---

## What It Does

### Phase 1 — Stockout Detection
Automatically detects out-of-stock events from daily sales data. Identifies duration, severity, and estimated lost units for every event across all stores and SKUs.

### Phase 2 — Substitution Mapping
When Product A goes out of stock, which products absorb the demand? The engine compares each potential substitute's baseline sales (14 days before stockout) against its sales during the OOS window to measure demand transfer. Each substitution pair receives an **elasticity score** (0–1) quantifying how reliably demand shifts.

### Phase 3 — Margin Impact Simulation
Every SKU has a price and margin percentage. When customers substitute, they often switch to a different price point — creating a margin shift. The simulator calculates:
- **Gross margin loss** from the stockout
- **Margin recovered** via substitution
- **Net financial impact** after accounting for the margin difference
- An interactive **capture rate slider** to model different replenishment scenarios

### Phase 4 — Replenishment Priority Scoring
A composite scoring algorithm (0–100) ranks every SKU-store combination by replenishment urgency, weighting five factors:
| Factor | Weight | Rationale |
|--------|--------|-----------|
| Net margin impact | 30% | Financial severity |
| OOS frequency | 20% | Chronic vs one-time |
| Substitution weakness | 20% | Poor subs = higher urgency |
| OOS duration | 15% | Longer events = worse |
| Volume impact | 15% | High-velocity SKUs matter more |

### Phase 5 — Predictive Intelligence
Flags items at risk of going out of stock in the next 7–14 days using:
- **Sales velocity trends** (7-day vs 30-day moving averages)
- **Recent zero-sales frequency**
- **Historical OOS patterns**
- **Declining demand signals**

For each at-risk item, the engine recommends replacements using a **product taxonomy** that understands product families:
- Cheddar Cheese 8oz → Mozzarella 8oz (same family, similar price) ✅
- Cheddar Cheese 8oz → Whole Milk 1gal (different family) ❌

Match scores are computed across four dimensions: product similarity (55pts max), price proximity (25pts), margin alignment (10pts), and historical elasticity (20pts).

---

## How It's Different

| Feature | Traditional Inventory Tools | Instacart | SubIntel |
|---------|---------------------------|-----------|----------|
| Stockout detection | ✅ Threshold-based | ❌ Not their scope | ✅ Pattern-based from sales data |
| Substitution mapping | ❌ | ✅ Customer-facing, real-time | ✅ Store operations, analytical |
| Margin impact modeling | ❌ | ❌ | ✅ Full financial simulation |
| Replenishment prioritization | Basic reorder points | ❌ | ✅ Weighted composite scoring |
| Predictive OOS risk | Some tools (Oracle, SAP) | ❌ | ✅ Velocity + history based |
| Product-aware replacements | ❌ | ✅ Based on acceptance data | ✅ Based on taxonomy + elasticity |
| Store-level granularity | Varies | Metro-level | ✅ Per store, per SKU |

**Key differentiator:** SubIntel operates *upstream* of the substitution moment. If the store never runs out, the substitution question never needs to be asked.

---

## Technical Details

- **Zero dependencies** — single HTML file, runs entirely in the browser
- **Offline capable** — no server, no API, no database
- **CSV upload** — bring your own data in the expected format
- **CSV export** — download any tab's data for further analysis
- **Interactive** — filters, sorting, scenario simulation, click-to-expand details
- **Canvas-based charts** — bar charts, donut charts, scatter plots, heatmaps, sparklines
- **Responsive** — works on desktop and tablet

### Data Format

Upload a CSV with these columns:

| Column | Type | Example |
|--------|------|---------|
| `date` | YYYY-MM-DD | 2025-10-01 |
| `store_id` | String | STR-101 |
| `store_name` | String | Downtown Metro |
| `sku_id` | String | SKU-001 |
| `sku_name` | String | Whole Milk 1gal |
| `category` | String | Dairy |
| `units_sold` | Integer | 24 (0 = out of stock) |
| `price` | Float | 4.29 |
| `margin_pct` | Float | 0.28 |

One row per SKU, per store, per day. `units_sold = 0` indicates an out-of-stock day.

---

## Data Source

Sample dataset derived from the [Kaggle Store Item Demand Forecasting Challenge](https://www.kaggle.com/c/demand-forecasting-kernels-only) — 10 stores, 50 items, 913,000 rows of daily sales data. Transformed with realistic grocery product names, categories, pricing, and margin data.

---

## Getting Started

1. **Open the dashboard:** [Live Demo](https://YOUR_USERNAME.github.io/subintel-dashboard/) or download `index.html` and open in any browser
2. **Explore sample data:** The dashboard loads with built-in sample data
3. **Upload your own data:** Click the "Data Upload" tab and drag in your CSV
4. **Simulate scenarios:** Use the capture rate slider on the Margin Simulator tab
5. **Export results:** Click "Export CSV" on any tab to download the data

---

## Project Structure

```
subintel-dashboard/
├── index.html          # Complete dashboard (single file, zero dependencies)
├── data/
│   └── sample_data.csv # Kaggle-derived sample dataset
└── README.md
```

---

## Built With

- Vanilla JavaScript (no frameworks)
- HTML5 Canvas for visualizations
- CSS3 with custom properties
- DM Sans + Space Mono typography

---

## Author

**Avanti** — Supply Chain Analytics

---

## License

MIT License — free to use, modify, and distribute.
