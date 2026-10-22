# Food Price Inflation in Kenya: Exploration & Forecasting

Exploratory analysis and time-series forecasting of Kenyan food-price inflation (2008-2026) using market-level data and the [`fpp3`](https://otexts.com/fpp3/) R ecosystem (`tsibble`, `feasts`, `fable`).

> **Status:** Work in progress. Results are preliminary. 
---

## Objectives

1. Describe the distribution and dynamics of 12-month food-price inflation across Kenyan markets and regions.
2. Compare national and regional patterns (Nairobi, Mombasa, Kisumu, Nakuru, Garissa, Wajir; six administrative regions).
3. Benchmark forecasting models (Mean, Naive, Seasonal Naive, ETS, ARIMA) on the national *Market Average* series.

## Data

| Item | Detail |
|---|---|
| Source | World Bank *Real-Time Food Prices (RTFP)* dataset, Kenya market file (`KEN_RTFP_mkt_2007_2026-10-05.csv`) |
| Size | 52,884 rows x 46 columns, 234 markets |
| Period | Jan 2008 to Oct 2026 (monthly) |
| Key variables | `mkt_name`, `adm1_name`, `price_date`, `c_food_price_index` (closing index), `inflation_food_price_index` (12-month % change), trust and confidence scores |
| Commodities | Beans, maize, goat (index weights per `components` column) |

The raw CSV is **not included** in this repository. Download it from the World Bank RTFP portal and place it in `data/`. Please check the dataset's licence and citation requirements before redistributing.

## Repository Structure

```
.
├── food_price_inflation.Rmd     # Full analysis (R Markdown)
├── food_price_inflation.pdf     # Knitted report
├── data/                        # Place the RTFP CSV here (git-ignored)
└── README.md
```

## Getting Started

**Requirements:** R >= 4.3 and the following packages:

```r
install.packages(c("fpp3", "tsibble", "dplyr", "lubridate", "ggplot2"))
```

**Run:**

1. Clone the repo and add the CSV to `data/`.
2. Update the file path in the first chunk:
   ```r
   df <- read.csv("data/KEN_RTFP_mkt_2007_2026-10-05.csv")
   ```
3. Knit `food_price_inflation.Rmd`.

## Methodology

1. **Cleaning:** parse dates, check for duplicate market-date records, convert to a monthly `tsibble` keyed by market.
2. **EDA:** histogram of inflation, national / regional / market time-series plots, seasonal plot, ranking of markets by mean inflation.
3. **Train/test split:** training data up to Apr 2025 (208 months); test set of the final 18 months.
4. **Models:** `MEAN`, `NAIVE`, `SNAIVE`, `ETS`, `ARIMA` fitted on the national Market Average series.
5. **Evaluation:** out-of-sample accuracy (RMSE, MAE, ME).

## Key Findings (preliminary)
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/5bcb637c-dbef-4543-87ba-b4038ed1c843" />

- **Skewed distribution:** Positively skewed with median of 1.5075, lower quartile = -5.8542 and upper quartile = 12.7692.
  
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/684938dd-96f3-4bbf-a8b7-6cabe61f2fb1" />


<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/fc218242-73fd-459e-9a3c-afc955190ca9" />
- **Regional differences:** pastoral and northern markets (North Eastern) show larger and more frequent swings than Nairobi and Central.



<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/196e20e1-de20-478b-b15d-a0e6feb32adf" />

- **Forecasting: (ARIMA, ETS, Mean, Naive and Seasonal Naive)** ETS(A,Ad,N) achieved the lowest test RMSE (about 10.3 pp), followed by ARIMA (14.6), Mean (20.3), Naive (49.6) and Seasonal Naive (62.1). ETS gains mainly from extrapolating the post-2024 rebound, and its prediction intervals are very wide.

| Model | RMSE | MAE | ME |
|---|---|---|---|
| ETS | 10.3 | 8.06 | -2.29 |
| ARIMA | 14.6 | 12.2 | 12.2 |
| Mean | 20.3 | 17.3 | -17.2 |
| Naive | 49.6 | 48.5 | 48.5 |
| Seasonal Naive | 62.1 | 59.1 | 59.1 |

- Forecasts from the four models


- Forecasts from the ARIMA model
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/1957666d-7fe7-4fb3-aea3-f5d1da84da0b" />


## Tools

R, R Markdown, `fpp3` (`tsibble`, `feasts`, `fable`), `ggplot2`, `dplyr`, `lubridate`.

## Author

**Val**, [GitHub profile](https://github.com/your-username)

## Licence

Code released under the MIT Licence (add a `LICENSE` file). Data remain subject to the original provider's terms.

## Acknowledgements

- World Bank Real-Time Food Prices (RTFP) dataset
- Hyndman & Athanasopoulos, [*Forecasting: Principles and Practice*](https://otexts.com/fpp3/)

