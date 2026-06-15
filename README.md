# Thesis Reproducibility Repository

This repository contains the notebooks and documentation for a thesis analysis of token-offering data, CoinMarketCap market data, crypto-asset liquidity, and liquidity-stress prediction.

The raw and processed data files are not stored directly in this repository because several CSV files are large. To reproduce the results, download the frozen data folders from the external data bundle and place them in the project root.

## Data Download

Download the `raw/` and `processed/` folders here:

**DATA LINK:** `<https://drive.google.com/drive/folders/12vudYN4L8ksR7rCS1ull1ac-sg3dtU0k?usp=sharing>`

After downloading, the project should look like this:

```text
thesis/
  README.md
  codebook.md
  codebook_cmc_token_level_panel.csv
  notebooks/
    01_TORD_clean.ipynb
    02_CMC_scrape_new.ipynb
    03_CMC_scrape_old.ipynb
    04_TORD_clean_first_offering.ipynb
    05_build_analysis_panel.ipynb
    06_selection_bias.ipynb
    07_analysis.ipynb
  raw/
    tord_v4.csv
    cmc_ohlcv_daily_raw.csv
    cmc_ohlcv_daily_extended_24m.csv
    cmc_token_metadata_raw.csv
    cmc_token_quotes_latest_raw.csv
  processed/
    tord_clean_min.csv
    tord_audit_before.csv
    tord_audit_after.csv
    tord_first_offering.csv
    cmc_ohlcv_combined.csv
    cmc_token_metadata.csv
    analysis_panel.csv
```

The CoinMarketCap extraction notebooks are included for transparency, but exact reproduction should use the frozen CSV snapshots in the downloaded `raw/` and `processed/` folders. Live extraction may produce different results because the CoinMarketCap API, token coverage, delistings, and rate limits can change over time.

## Repository Contents

`notebooks/01_TORD_clean.ipynb` cleans the raw TORD token-offering file and creates the cleaned TORD outputs.

`notebooks/02_CMC_scrape_new.ipynb` documents the main CoinMarketCap scraping workflow for the recent 24-month daily OHLCV window.

`notebooks/03_CMC_scrape_old.ipynb` documents the earlier CoinMarketCap extraction workflow used for the extended historical OHLCV snapshot.

`notebooks/04_TORD_clean_first_offering.ipynb` collapses TORD to one offering per token and combines the old and new CoinMarketCap OHLCV snapshots.

`notebooks/05_build_analysis_panel.ipynb` builds the final token-day analysis panel and constructs liquidity, activity, stress, crash, and market-factor variables.

`notebooks/06_selection_bias.ipynb` evaluates sample attrition and selection/survivorship patterns.

`notebooks/07_analysis.ipynb` runs the main descriptive, econometric, machine-learning, calibration, and portfolio analyses.

`codebook.md` describes the final merged token-day panel.

`codebook_cmc_token_level_panel.csv` provides a compact variable-level codebook.

## Environment

The notebooks were developed in Python with the following main packages:

```text
pandas
numpy
matplotlib
scipy
statsmodels
scikit-learn
requests
python-dotenv
xgboost
linearmodels
shap
openpyxl
jupyter
```

One way to create a local environment is:

```bash
python -m venv .venv
pip install pandas numpy matplotlib scipy statsmodels scikit-learn requests python-dotenv xgboost linearmodels shap openpyxl jupyter
```

If rerunning CoinMarketCap extraction notebooks, create a local `.env` file with:

```env
CMC_API_KEY=your_coinmarketcap_key_here
```

## Reproduction Workflow

For exact reproduction of the thesis results, first download the frozen `raw/` and `processed/` folders from the data link above.

Recommended working directory: run notebooks with the current working directory set to `notebooks/`. Several notebooks use paths such as `../raw` and `../processed`.

Run the notebooks in this order:

1. `01_TORD_clean.ipynb`
2. `04_TORD_clean_first_offering.ipynb`
3. `05_build_analysis_panel.ipynb`
4. `06_selection_bias.ipynb`
5. `07_analysis.ipynb`

Notebooks `02_CMC_scrape_new.ipynb` and `03_CMC_scrape_old.ipynb` are included to document the data extraction process. They are not required for exact reproduction if the frozen data bundle has already been downloaded.

If only reproducing the final analyses from the already-built panel, start from:

1. `06_selection_bias.ipynb`
2. `07_analysis.ipynb`

## Outputs

The main generated dataset is:

```text
processed/analysis_panel.csv
```

The analysis notebooks save figures into the `notebooks/` folder when run.

## Notes

The downloaded data bundle is part of the reproducibility package. Without it, the repository contains the code and documentation, but not the large CSV inputs needed to recreate the exact reported results.