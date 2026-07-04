# apexplanet-data-analytics

**Task 1: Foundational Setup & Exploratory Data Analysis (EDA)**
Timeline: Day 1–6 · Dataset: **Superstore Sales**

## Repository Structure
```
apexplanet-data-analytics/
├── dashboards/
│   └──Sales_Dashboard.pbix
│   └──SALES_DASHBOARD.png
├── data/
│   ├── Sample-Superstore.csv     # raw dataset (9,994 orders, 2015-2018)
│   ├── superstore_clean.csv      # cleaned + feature-engineered dataset
├── notebooks/
│   └── 01_EDA.ipynb              # full exploratory data analysis
├── reports/
│   └── figures/                  # exported PNG charts from the EDA
├── scripts/
│   ├── clean_data.py             # reusable cleaning pipeline
│   └── build_notebook.py         # generates the notebook programmatically
└── README.md
```

## Environment Setup (Day 1–2)
```bash
# Create environment (conda or venv both work)
python3 -m venv venv && source venv/bin/activate

# Install dependencies
pip install pandas numpy matplotlib seaborn plotly sqlalchemy openpyxl jupyter
```

**Dataset:** Superstore Sales (US retail orders, Jan 2015 – Dec 2018, 9,994 rows, 21 columns:
order/ship info, customer/segment, geography, product, sales, quantity, discount, profit).

## Day 3–4: Data Cleaning
Run:
```bash
python scripts/clean_data.py
```
This:
- Standardizes column names to snake_case
- Parses `order_date` / `ship_date` to datetime
- Casts `postal_code` to nullable Int (11 missing values kept — not needed for sales/profit analysis)
- Drops exact duplicate rows (0 found — dataset was already clean)
- Engineers: `order_year`, `order_month`, `order_year_month`, `shipping_days`, `profit_margin`, `is_loss`
- Writes `data/superstore_clean.csv`

## Day 5–6: Exploratory Data Analysis
Open `notebooks/01_EDA.ipynb` (already executed, outputs included) or re-run:
```bash
jupyter nbconvert --to notebook --execute --inplace notebooks/01_EDA.ipynb
```

### Key Findings
| Metric | Value |
|---|---|
| Total Sales | $2,297,200.86 |
| Total Profit | $286,397.02 |
| Overall Profit Margin | 12.47% |
| Orders sold at a loss | 18.72% |
| Avg. shipping time | 3.96 days |
| Discount ↔ Profit correlation | -0.219 |

- **Furniture drags down profit** — `Tables` (-$17.7K) and `Bookcases` (-$3.5K) are the two biggest loss-making sub-categories.
- **Technology drives profit** — `Copiers`, `Phones`, and `Accessories` are the top 3 profit generators.
- **Discounting is risky** — heavier discounts correlate with lower (often negative) profit, especially in Furniture.
- **California & New York** lead in sales; **Texas** has strong sales but weak profit — worth a deeper discount/cost review.
- Sales and profit both show clear seasonal peaks toward Q4 (Nov/Dec) each year.

Full charts are in `reports/figures/` (8 PNGs) and inline in the notebook.

## Next (Task 2+)
- Deeper customer segmentation (RFM)
- Discount-threshold policy investigation for Furniture / Texas
- Interactive dashboard in `dashboards/`
