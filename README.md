# Ramen: Walmart Hurricane Analytics

DS2002 Fall 2026 Midterm Project. Team Ramen: Antony Lin ([antonylin0428](https://github.com/antonylin0428)) and Danny Zhang ([zhang-dny](https://github.com/zhang-dny)).

We act as Walmart's data science consultants for Hurricane Helene (landfall near Perry, FL on Sept 26, 2024). The project cleans five messy data files, adds real weather and the real storm track, and answers five questions about the surge in Florida store sales.

## Headline results

| Question | Answer |
|---|---|
| Q1 Demand surge | Orders ran 2.4x normal in the 7 days before landfall (Sept 19-25 vs Sept 1-15). Breakfast (all Pop-Tarts) rose most, +414% |
| Q2 SKU consolidation | 45 SKUs became 20 products. Pop-Tarts went from the best single SKU at #8 of 45 to the #2 product of 20 |
| Q3 Weather | Orders lead Orlando's peak wind by 2 days (r = 0.84, n = 28 days) |
| Q4 Stores | No geographic pattern: all 12 stores surged 2.2x-2.5x, none is an outlier |
| Q5 Board games | The +250% is a peak-days figure; Monopoly-type games rose more than the basket, playing cards did not. Stock extra, sized like the basket |

## How to run

1. Open `Midterm_teamRamen_notebook.ipynb` in Google Colab:
   https://colab.research.google.com/github/zhang-dny/walmart-hurricane-project/blob/main/Midterm_teamRamen_notebook.ipynb
2. Choose **Runtime > Restart session and run all**. It takes about a minute.
3. The notebook clones this repo, so the `data/` folder is found automatically. It needs internet access for the live weather call and prints which weather source it used.

To run locally instead:

```bash
git clone https://github.com/zhang-dny/walmart-hurricane-project.git
cd walmart-hurricane-project
python -m pip install pandas numpy matplotlib requests jupyter
jupyter notebook Midterm_teamRamen_notebook.ipynb
```

Libraries: `pandas`, `numpy`, `matplotlib`, `requests`, `sqlite3` (all preinstalled in Colab).

## Repository layout

```
walmart-hurricane-project/
├── CHARTER.md                   team charter
├── README.md                    this file
├── Midterm_teamRamen_notebook.ipynb                     the final deliverable (cleaning, Q1-Q5, reflection)
├── Walmart Hurricane Analytics Team Ramen Midterm.pptx   presentation slides
├── data/
│   ├── walmart_transactions.csv     supplied: ~25,000 POS records
│   ├── store_locations.csv          supplied: Florida store metadata
│   ├── product_catalog.csv          supplied: SKU master list
│   ├── distribution_centers.csv     supplied: distribution centers
│   ├── inventory_and_sales.db       supplied: SQLite (daily_sales_summary, inventory_levels)
│   └── helene_best_track.csv        added: Helene storm track from NOAA NHC
└── notebooks/
    ├── 2026-10-23 - Midterm Checkpoint - Lab.ipynb
    ├── 2026-10-30 - Midterm Progress - Lab.ipynb
    └── scratch/
```

## Data sources

- **Supplied data:** the five files above. They contain intentional quality problems; every cleaning decision, with the rows it touched, is in the notebook's decision log (section 8).
- **Weather (live API):** Open-Meteo historical archive API, base URL `https://archive-api.open-meteo.com/v1/archive` (free, no key). Orlando (28.5383, -81.3792), daily precipitation, maximum wind speed and mean surface pressure, in America/New_York calendar days. If the call fails, the notebook falls back to a saved copy of the same data from the course studio and labels the source `fallback sample`.
- **Storm track:** NOAA National Hurricane Center best track for Helene (AL092024), saved as `data/helene_best_track.csv`. Source file: https://ftp.nhc.noaa.gov/atcf/archive/2024/bal092024.dat.gz. Used to measure each store's distance from the storm path (Q4).

## Reflection

Both team members answer all five questions in section 9 of the notebook. Short versions:

**1. Data quality impact.** Danny: Pop-Tarts were split across four SKU codes; combined, they go from #8 of 45 SKUs to #2 of 20 products. Duplicate store rows would have inflated the order table by 17%. Antony: dates like 06/09/2024 can be read as June 9 instead of Sept 6, which would move transactions in or out of the 7-day pre-landfall window.

**2. ETL trade-offs.** Danny: the Q1 headline depends on the baseline window (2.36x for Sept 1-15, 2.23x for Sept 1-18, 3.08x for the peak days only), so every number is shown with its window. Antony: using Orlando weather for all 12 stores is a limitation; per-store weather would make Q3 and Q4 more reliable.

**3. Pipeline trust.** We agree with the CDO. Danny: dates and joins were the most fragile, so the notebook checks date ranges and row counts after every join. Antony: the warehouse and transaction data disagree on Entertainment (+267% vs +232% on the peak days), so sources need validating before a business decision.

**4. Business vs. data.** Stock extra board games, sized like the overall basket: they don't spoil, and a stockout during a storm loses the sale. With more data we would check several past hurricanes.

**5. Team collaboration.** Danny led the cleaning and analysis; Antony worked on interpreting results and the conclusions. Next time we would split ownership by question from week one.

## Notes and limitations

- Tallahassee (FL-619) has no coordinates in the store file; they were filled from the city center and logged.
- Weather is from one location (Orlando) and compared with all stores' orders; the correlation uses 28-30 daily points around a single storm.
- The data is synthetic, mirroring the real 2004 Walmart hurricane case.

See `CHARTER.md` for roles.
