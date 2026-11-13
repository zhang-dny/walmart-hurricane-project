# Ramen: Walmart Hurricane Analytics
 
DS2002 Fall 2026 Midterm Project. Team Ramen: Antony Lin ([antonylin0428](https://github.com/antonylin0428)) and Danny ([zhang-dny](https://github.com/zhang-dny)).
 
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
Libraries: `pandas`, `numpy`, `matplotlib`, `requests`, `sqlite3` (all preinstalled in Colab).
 
## Repository layout
 
```
walmart-hurricane-project/
├── CHARTER.md                   team charter
├── README.md                    this file
├── data/
│   ├── walmart_transactions.csv     supplied: ~25,000 POS records
│   ├── store_locations.csv          supplied: Florida store metadata
│   ├── product_catalog.csv          supplied: SKU master list
│   ├── distribution_centers.csv     supplied: distribution centers
│   ├── inventory_and_sales.db       supplied: SQLite (daily_sales_summary, inventory_levels)
│   └── helene_best_track.csv        added: Helene storm track from NOAA NHC
└── notebooks/
    ├── team_notebook.ipynb          the final deliverable (cleaning, Q1-Q5, reflection)
    ├── 2026-10-23 - Midterm Checkpoint - Lab.ipynb
    ├── 2026-10-30 - Midterm Progress - Lab.ipynb
    └── scratch/
```
 
## Data sources
 
- **Supplied data:** the five files above. They contain intentional quality problems; every cleaning decision, with the rows it touched, is in the notebook's decision log (section 8).
- **Weather (live API):** Open-Meteo historical archive API, base URL `https://archive-api.open-meteo.com/v1/archive` (free, no key). Orlando (28.5383, -81.3792), daily precipitation, maximum wind speed and mean surface pressure, in America/New_York calendar days. If the call fails, the notebook falls back to a saved copy of the same data from the course studio and labels the source `fallback sample`.
- **Storm track:** NOAA National Hurricane Center best track for Helene (AL092024), saved as `data/helene_best_track.csv`. Source file: https://ftp.nhc.noaa.gov/atcf/archive/2024/bal092024.dat.gz. Used to measure each store's distance from the storm path (Q4).
## Notes and limitations
 
- Tallahassee (FL-619) has no coordinates in the store file; they were filled from the city center and logged.
- Weather is from one location (Orlando) and compared with all stores' orders; the correlation uses 28-30 daily points around a single storm.
- The data is synthetic, mirroring the real 2004 Walmart hurricane case.
 
