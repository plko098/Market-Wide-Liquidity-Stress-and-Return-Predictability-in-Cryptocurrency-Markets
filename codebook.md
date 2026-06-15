Below is a thesis-ready codebook for the **final merged token-day panel**. It is written to match the dataset structure you showed: a daily panel with one row per `cmc_id` and `date`, combining CoinMarketCap market data, CoinMarketCap token metadata, and token-offering characteristics from TORD.

You can place this in an appendix or in the data section of your methodology chapter.

---

# Codebook: Final Merged Token-Day Panel

## Dataset overview

The final analysis dataset is a **token-day panel** in which each observation corresponds to one token (`cmc_id`) on one calendar day (`date`). The panel merges three data sources:

1. **CoinMarketCap daily market data**, providing daily OHLCV and market capitalization.
2. **CoinMarketCap token metadata**, providing token identity, platform, supply, and market-structure information.
3. **TORD (Token Offering Research Database) offering data**, providing token-sale, project, and issuance characteristics.

Daily market variables vary over time. Token metadata and TORD variables are largely static and are repeated across daily observations for a given token.

## Unit of observation

One row = one **token-day** observation.

## Panel dimensions

* **Token identifier:** `cmc_id`
* **Time identifier:** `date`
* **Observed sample:** 1,853 tokens over 725 days
* **Date range:** 2024-04-01 to 2026-03-30

## Important notes

* Variables originating from TORD are **offering-level variables collapsed to token level** and then merged into the daily panel.
* Variables from CoinMarketCap metadata and latest quote snapshots are **token-level or near-static** and repeated across days.
* Some TORD variables have substantial missingness due to incomplete reporting in the original offering data.
* Some engineered variables, especially `distributed_in_ico_frac` and `ico_duration_days`, may require additional plausibility screening before use in econometric models.
* In the merged file, `name_x` and `name_y` reflect token/project names from different sources:

  * `name_x`: CoinMarketCap name
  * `name_y`: TORD name

---

# Variable-by-variable codebook

## 1. Panel identifiers and daily market variables

| Variable     | Source                          |     Level | Description                                | Unit | Notes                              |
| ------------ | ------------------------------- | --------: | ------------------------------------------ | ---- | ---------------------------------- |
| `cmc_id`     | CoinMarketCap / merge key       | Token-day | Unique token identifier from CoinMarketCap | ID   | Primary join key across datasets   |
| `date`       | CoinMarketCap                   | Token-day | Observation date                           | Date | Daily UTC-based calendar date      |
| `open`       | CoinMarketCap daily market data | Token-day | Opening price on that day                  | USD  | Daily opening price                |
| `high`       | CoinMarketCap daily market data | Token-day | Highest observed price on that day         | USD  | Daily high price                   |
| `low`        | CoinMarketCap daily market data | Token-day | Lowest observed price on that day          | USD  | Daily low price                    |
| `close`      | CoinMarketCap daily market data | Token-day | Closing price on that day                  | USD  | Daily closing price                |
| `volume`     | CoinMarketCap daily market data | Token-day | Daily trading volume                       | USD  | Dollar trading volume for the day  |
| `market_cap` | CoinMarketCap daily market data | Token-day | Daily market capitalization                | USD  | Usually price × circulating supply |

---

## 2. CoinMarketCap identity and metadata variables

| Variable                 | Source                 | Level | Description                                                    | Unit            | Notes                                                    |
| ------------------------ | ---------------------- | ----: | -------------------------------------------------------------- | --------------- | -------------------------------------------------------- |
| `name_x`                 | CoinMarketCap metadata | Token | Token/project name according to CoinMarketCap                  | Text            | CMC source name                                          |
| `symbol`                 | CoinMarketCap metadata | Token | Token ticker symbol according to CoinMarketCap                 | Text            | Example: BTC, ETH                                        |
| `slug`                   | CoinMarketCap metadata | Token | URL-friendly CoinMarketCap asset identifier                    | Text            | Useful for referencing CMC listings                      |
| `date_added`             | CoinMarketCap metadata | Token | Date the asset was added to CoinMarketCap                      | Date            | CMC platform listing date, not necessarily issuance date |
| `category`               | CoinMarketCap metadata | Token | Broad CoinMarketCap category label                             | Text            | Often “coin” or “token,” but may vary                    |
| `tags`                   | CoinMarketCap metadata | Token | Comma- or pipe-separated list of descriptive tags              | Text            | Examples may include DeFi, gaming, meme, AI              |
| `platform_id`            | CoinMarketCap metadata | Token | Numeric identifier of the underlying platform                  | ID              | Relevant for tokens issued on another blockchain         |
| `platform_name`          | CoinMarketCap metadata | Token | Name of the platform blockchain                                | Text            | Example: Ethereum, BNB, Solana                           |
| `platform_symbol`        | CoinMarketCap metadata | Token | Ticker symbol of the platform blockchain                       | Text            | Example: ETH, BNB                                        |
| `platform_slug`          | CoinMarketCap metadata | Token | URL-friendly name of the platform blockchain                   | Text            | Useful for CMC referencing                               |
| `platform_token_address` | CoinMarketCap metadata | Token | Token address on the underlying platform                       | Text            | Primarily relevant for smart-contract tokens             |
| `is_hidden`              | CoinMarketCap metadata | Token | Indicator for whether CMC hides the token from normal listings | 0/1             | May flag inactive/problematic tokens                     |
| `description_len`        | CoinMarketCap metadata | Token | Length of the CoinMarketCap description text                   | Character count | Proxy for how much descriptive information is available  |

---

## 3. CoinMarketCap latest snapshot variables

These variables come from the CoinMarketCap “latest quote” endpoint and represent snapshot values at the time of scraping, not a historical daily time series.

| Variable              | Source                     | Level | Description                                        | Unit        | Notes                                   |
| --------------------- | -------------------------- | ----: | -------------------------------------------------- | ----------- | --------------------------------------- |
| `price_latest`        | CoinMarketCap latest quote | Token | Latest token price at scrape time                  | USD         | Snapshot variable                       |
| `volume_24h_latest`   | CoinMarketCap latest quote | Token | 24-hour trading volume at scrape time              | USD         | Snapshot variable                       |
| `market_cap_latest`   | CoinMarketCap latest quote | Token | Market capitalization at scrape time               | USD         | Snapshot variable                       |
| `circulating_supply`  | CoinMarketCap latest quote | Token | Tokens currently in public circulation             | Token units | Snapshot variable                       |
| `total_supply`        | CoinMarketCap latest quote | Token | Total number of tokens created so far              | Token units | Snapshot variable                       |
| `max_supply`          | CoinMarketCap latest quote | Token | Maximum number of tokens that can exist            | Token units | Missing if uncapped                     |
| `cmc_rank`            | CoinMarketCap latest quote | Token | CoinMarketCap rank based on relative market size   | Rank        | Lower values indicate larger assets     |
| `num_market_pairs`    | CoinMarketCap latest quote | Token | Number of trading pairs/markets listed on CMC      | Count       | Proxy for market access and tradability |
| `last_updated_latest` | CoinMarketCap latest quote | Token | Timestamp when latest snapshot values were updated | Timestamp   | Snapshot update time                    |

---

## 4. TORD project identity and offering variables

These variables originate in the cleaned TORD offering dataset. In the final panel they are token-level variables repeated across all daily observations for the same `cmc_id`.

| Variable             | Source | Level | Description                                                       | Unit          | Notes                                      |
| -------------------- | ------ | ----: | ----------------------------------------------------------------- | ------------- | ------------------------------------------ |
| `country`            | TORD   | Token | Country associated with the project                               | Text          | Often missing or inconsistently reported   |
| `is_ico`             | TORD   | Token | Indicator for Initial Coin Offering                               | 0/1           | Collapsed from offering-level data         |
| `is_ieo`             | TORD   | Token | Indicator for Initial Exchange Offering                           | 0/1           | Collapsed from offering-level data         |
| `is_sto`             | TORD   | Token | Indicator for Security Token Offering                             | 0/1           | Collapsed from offering-level data         |
| `ico_start`          | TORD   | Token | Start date of the token offering                                  | Date          | May contain noisy or implausible dates     |
| `ico_end`            | TORD   | Token | End date of the token offering                                    | Date          | Used to derive duration                    |
| `price_usd`          | TORD   | Token | Token sale price                                                  | USD per token | Offering-stage price                       |
| `raised_usd`         | TORD   | Token | Total funds raised in the token sale                              | USD           | Highly skewed and often missing            |
| `distributed_in_ico` | TORD   | Token | Share of tokens distributed in the ICO                            | Raw numeric   | Raw source may mix percents and fractions  |
| `sold_tokens`        | TORD   | Token | Number of tokens actually sold                                    | Token units   | Often missing                              |
| `token_for_sale`     | TORD   | Token | Number of tokens made available for sale                          | Token units   | Often missing                              |
| `whitelist`          | TORD   | Token | Indicator for whitelist requirement                               | 0/1           | Offering participation rule                |
| `kyc`                | TORD   | Token | Indicator for Know-Your-Customer requirement                      | 0/1           | Identity verification at token sale        |
| `bonus`              | TORD   | Token | Indicator for bonus/discount offering                             | 0/1           | Promotional sale feature                   |
| `restricted_areas`   | TORD   | Token | Jurisdictions excluded from the sale                              | Text          | Unstructured text                          |
| `min_investment`     | TORD   | Token | Raw minimum investment requirement                                | Text          | Original uncleaned field                   |
| `bounty`             | TORD   | Token | Indicator for bounty program                                      | 0/1           | Promotional / marketing activity           |
| `mvp`                | TORD   | Token | Indicator for Minimum Viable Product at offering                  | 0/1           | Proxy for project maturity                 |
| `pre_ico_start`      | TORD   | Token | Presale start date                                                | Date          | Presence also used to define `has_presale` |
| `pre_ico_end`        | TORD   | Token | Presale end date                                                  | Date          | Presale timing variable                    |
| `pre_ico_price_usd`  | TORD   | Token | Presale token price                                               | USD per token | Often missing                              |
| `platform`           | TORD   | Token | Indicator for whether project is classified as a platform project | 0/1           | TORD-specific project classification       |
| `accepting`          | TORD   | Token | Payment methods accepted in the token sale                        | Text          | Example: ETH, BTC, fiat                    |
| `link_white_paper`   | TORD   | Token | Whitepaper URL                                                    | Text / URL    | Used to construct `has_whitepaper`         |
| `linkedin_link`      | TORD   | Token | LinkedIn URL                                                      | Text / URL    | Used to construct `has_linkedin`           |
| `github_link`        | TORD   | Token | GitHub URL                                                        | Text / URL    | Used to construct `has_github`             |
| `website`            | TORD   | Token | Official website URL                                              | Text / URL    | Used to construct `has_website`            |
| `rating`             | TORD   | Token | External ICO/project rating                                       | Score         | Subjective third-party rating              |
| `teamsize`           | TORD   | Token | Number of team members                                            | Count         | Excludes advisors where possible           |
| `ERC20`              | TORD   | Token | Indicator for ERC-20 token standard                               | 0/1           | Ethereum token standard                    |
| `contract_address`   | TORD   | Token | Token smart contract address                                      | Text          | Mostly relevant for on-chain tokens        |
| `Source`             | TORD   | Token | Original source label for the offering record                     | Text          | Example: ICObench                          |
| `name_y`             | TORD   | Token | Project/token name according to TORD                              | Text          | TORD source name                           |
| `token`              | TORD   | Token | Token ticker according to TORD                                    | Text          | TORD source ticker                         |

---

## 5. Engineered and cleaned TORD variables

These variables were created during cleaning to standardize messy raw TORD fields and improve usability.

### 5.1 Cleaned fraction and threshold variables

| Variable                  | Source               | Level | Description                                                        | Unit                    | Notes                                                                                           |
| ------------------------- | -------------------- | ----: | ------------------------------------------------------------------ | ----------------------- | ----------------------------------------------------------------------------------------------- |
| `distributed_in_ico_frac` | Engineered from TORD | Token | Standardized fraction of tokens distributed in the ICO             | Fraction                | Intended to be on 0–1 scale, though implausible values may remain due to source inconsistencies |
| `min_investment_raw`      | Engineered from TORD | Token | Original raw minimum investment text preserved for audit           | Text                    | Raw text version                                                                                |
| `min_inv_clean`           | Engineered from TORD | Token | Cleaned/normalized version of the minimum investment string        | Text                    | Standardized formatting                                                                         |
| `min_inv_pairs`           | Engineered from TORD | Token | Structured value-unit pairs extracted from minimum investment text | Text / JSON-like string | Supports fields such as “100 USD” or “0.1 ETH”                                                  |
| `min_inv_min_value`       | Engineered from TORD | Token | Smallest numeric minimum investment detected                       | Numeric                 | Not directly comparable across different units                                                  |
| `min_inv_min_unit`        | Engineered from TORD | Token | Unit associated with the smallest detected minimum investment      | Text                    | Example: USD, ETH                                                                               |
| `min_inv_usd_value`       | Engineered from TORD | Token | Minimum investment expressed in USD when directly available        | USD                     | No conversion from non-USD token units unless explicitly possible                               |

### 5.2 Flags related to minimum investment parsing

| Variable                   | Source               | Level | Description                                                            | Unit | Notes                                |
| -------------------------- | -------------------- | ----: | ---------------------------------------------------------------------- | ---- | ------------------------------------ |
| `flag_min_inv_multi`       | Engineered from TORD | Token | Indicator for multiple minimum investment options detected             | 0/1  | Example: “1 ETH or 0.1 BTC”          |
| `flag_min_inv_ratio`       | Engineered from TORD | Token | Indicator for ratio- or condition-like formatting in investment text   | 0/1  | Signals harder-to-interpret patterns |
| `flag_min_inv_equals`      | Engineered from TORD | Token | Indicator for equality-style formatting detected                       | 0/1  | Parsing warning flag                 |
| `flag_min_inv_missinglike` | Engineered from TORD | Token | Indicator that the minimum investment field looked effectively missing | 0/1  | Example: “N/A”, “-”, “none”          |

### 5.3 Time and disclosure engineering

| Variable            | Source               | Level | Description                                    | Unit | Notes                                                                        |
| ------------------- | -------------------- | ----: | ---------------------------------------------- | ---- | ---------------------------------------------------------------------------- |
| `ico_duration_days` | Engineered from TORD | Token | Number of days between ICO start and end dates | Days | May contain implausible negative or extreme values if source dates are noisy |
| `has_presale`       | Engineered from TORD | Token | Indicator for the existence of a presale       | 0/1  | Based on presale date availability                                           |
| `has_whitepaper`    | Engineered from TORD | Token | Indicator for whitepaper link availability     | 0/1  | Disclosure/professionalization proxy                                         |
| `has_github`        | Engineered from TORD | Token | Indicator for GitHub link availability         | 0/1  | Technical transparency proxy                                                 |
| `has_linkedin`      | Engineered from TORD | Token | Indicator for LinkedIn link availability       | 0/1  | Organizational presence proxy                                                |
| `has_website`       | Engineered from TORD | Token | Indicator for website availability             | 0/1  | Basic project disclosure proxy                                               |

### 5.4 Data-quality flags

| Variable                    | Source               | Level | Description                                               | Unit | Notes                                  |
| --------------------------- | -------------------- | ----: | --------------------------------------------------------- | ---- | -------------------------------------- |
| `flag_suspicious_ico_start` | Engineered from TORD | Token | Indicator for an implausible or suspicious ICO start date | 0/1  | Based on pre-defined plausibility rule |
| `flag_negative_price`       | Engineered from TORD | Token | Indicator that offering price was negative                | 0/1  | Data-quality flag                      |
| `flag_negative_raised`      | Engineered from TORD | Token | Indicator that total funds raised was negative            | 0/1  | Data-quality flag                      |

---

# Interpretation guidance

## Daily variables

These are the main variables used to construct time-varying outcomes and predictors:

* returns,
* volatility,
* turnover,
* illiquidity,
* stress labels.

The key daily variables are:

* `close`
* `volume`
* `market_cap`
* and OHLC values for volatility and spread estimation.

## Token-level metadata

These variables describe the token’s broader identity and market structure:

* whether it is a coin or token,
* what blockchain it belongs to,
* how many market pairs it has,
* and what its supply structure looks like.

These are especially useful as controls or grouping variables.

## TORD variables

These capture **pre-market and issuance-stage characteristics**:

* sale type,
* fundraising structure,
* governance features,
* project disclosure,
* and project quality proxies.

They are especially relevant for testing whether issuance characteristics are related to later liquidity or market fragility.

---

# Variables likely to require extra cleaning or caution

The following variables should be used with care and may require screening, winsorization, or recoding:

* `distributed_in_ico_frac`
* `ico_duration_days`
* `raised_usd`
* `price_usd`
* `min_inv_min_value`
* `min_inv_usd_value`
* `rating`

Reasons include:

* source inconsistency,
* heavy skewness,
* high missingness,
* and occasionally implausible values.

---

# Suggested derived variables for the analysis dataset

These are not yet part of the master merged panel, but are natural next-step variables for the thesis:

* `ret_1d`: daily simple return from `close`
* `log_ret_1d`: daily log return
* `dollar_volume`: daily trading value, typically proxied by `volume`
* `turnover`: `volume / market_cap`
* `amihud_illiquidity`: `|return| / dollar_volume`
* rolling return volatility
* OHLC-based spread estimator
* zero-volume indicator
* liquidity-stress indicator
* market-wide liquidity factor

---

The final dataset is a merged token-day panel in which daily CoinMarketCap market variables are combined with static CoinMarketCap token metadata and token-offering characteristics from TORD. Market variables vary over time at the token-day level, while metadata and offering characteristics are repeated across daily observations for the same token. The final panel contains 1,026,525 observations across 1,853 tokens from 1 April 2024 to 30 March 2026.