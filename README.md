# Long Short-Term Memory Networks versus Volatility Models for Rupiah Forecasting

Code and data for the article of the same title by Sry Yulianti Lobo, Rahmat Syam, and Wahidah Sanusi
(Universitas Negeri Makassar).

The study tests whether a multivariate long short-term memory (LSTM) network outperforms an
ARIMAX(1)-GARCH(1,1) benchmark and two random walks (without and with drift) in forecasting the daily
IDR/USD rate (JISDOR) one day ahead. Both models receive the same inputs, namely the log return and the
first differences of the Federal Funds Rate, the Bank Indonesia policy rate, and the Indonesian CPI. Every
model is re-estimated on 1,003 fixed-size rolling windows, and the comparisons are tested with the
Clark-West statistic for nested pairs and the Diebold-Mariano statistic with the Harvey-Leybourne-Newbold
correction for non-nested pairs.

## Contents

| File | Description |
|------|-------------|
| `LSTM_vs_ARIMAX_GARCH_IDRUSD_Rolling.ipynb` | Main notebook with outputs. Reproduces every table and figure of the article. |
| `merged_data_clean.csv` | Daily data `date, fx, ffr, cpi, bi` from 2 April 2018 to 27 July 2026 (2,004 trading days). |
| `forecasts_k1000.csv` | One-day-ahead forecasts of all four models on 1,003 origins (window 1,000). |
| `forecasts_common_k750.csv`, `forecasts_common_k1250.csv` | Forecasts with windows of 750 and 1,250 on the 753 common origins. |
| `forecasts_k1000_u64.csv` | Forecasts with a 64-unit LSTM. |
| `forecasts_k1000_seed7.csv` | LSTM forecasts with a second seed on 201 origins. |
| `figures/` | Article figures at 600 dpi (PNG and TIFF) and as vector PDF. |
| `requirements.txt` | Python package versions used for the article. |
| `LICENSE` | MIT license. |

The forecast files let every table be recomputed in seconds. The notebook reads them and skips any
origin that is already stored, so the rolling estimation only runs for origins that are missing.

## Data

| Column | Variable | Source |
|--------|----------|--------|
| `fx` | JISDOR IDR/USD rate | Bank Indonesia |
| `ffr` | Federal Funds target range, upper limit | FRED series DFEDTARU |
| `bi` | BI policy rate | Bank Indonesia |
| `cpi` | Indonesian CPI, reconstructed by chaining monthly inflation | Statistics Indonesia (BPS) |

JISDOR is recorded only on Indonesian trading days. The policy rates are carried forward from each
announcement to the next. The CSV stores the CPI by its reference month. Step 2 of the notebook moves the
CPI of month M to its BPS release date, which is the first trading day of month M+1 except for the
December 2025 CPI released on 5 January 2026, so no value is used before it is published.

## How to run

Open the notebook in Google Colab with a CPU runtime, put `merged_data_clean.csv` and the forecast files
in the working folder, and run the cells in order. Step 0 optionally uses a Google Drive folder as the
working folder, so an interrupted run can resume from the stored forecasts. Without the forecast files,
Step 10 takes about 2.5 hours and Steps 13 and 17 take several hours each on a Colab CPU.

## Notebook steps and article outputs

| Step | Output in the article |
|------|-----------------------|
| 0 | Working folder on Google Drive (optional) |
| 1 to 3 | Data import, CPI placement by release date, log returns |
| 4, 5 | Unit-root and Johansen tests |
| 6, 7 | Order selection, ARIMAX(1)-GARCH(1,1) estimates and diagnostics (Table I, Section III.A) |
| 8, 9 | LSTM construction and hyperparameter selection (Table II) |
| 10 | Rolling-window forecasts of the four models |
| 11 | Accuracy metrics (Table III) |
| 12 | Clark-West and Diebold-Mariano-HLN tests (Tables IV and V) |
| 13 | Robustness to the window length (Table VII) |
| 14 | Cumulative squared-error difference (Figure 2) |
| 15 | Subperiods and bias-variance statistics (Section III.C, Table VIII) |
| 16, 17 | Sensitivity of the ARIMAX-GARCH and the LSTM (Tables VI and VIII, Section III.C) |
| 18 | Cross-window distribution of the ARIMAX-GARCH estimates (Table I, last column) |
| 19 | Descriptive figures of the rate, the returns, and the last 120 forecasts |
| 20 | Supporting statistics quoted in the text (yearly random-walk MAPE, nonzero regressor days) |
| 21 | Article figures in high resolution (Figures 1 and 2) |
| 22 | Package versions (`requirements.txt`) |

## Method notes

- Model orders and LSTM hyperparameters are selected on the first window only (3 April 2018 to
  19 May 2022) and then frozen.
- Every input of the LSTM is min-max scaled with the minimum and maximum of its own window.
- The one-step ARIMAX-GARCH forecast sets the unknown future regressor differences to zero.
- Random seeds are fixed (42 for the main LSTM, 7 for the seed check).

## Citation

S. Y. Lobo, R. Syam, and W. Sanusi, "Long short-term memory networks versus volatility models for rupiah
forecasting," submitted to J. Appl. Inform. Comput., 2026.

## License

MIT (see `LICENSE`).
