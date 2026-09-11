# Netflix content growth & forecast dashboard

A Power BI dashboard that analyzes Netflix's content catalog (2008-2021) and
forecasts near-term growth in new titles added per month, combining a
Prophet time-series model with an AI-generated narrative summary.

## What it does

- Cleans and prepares the [Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows)
  Kaggle dataset (~8,800 titles)
- Builds a monthly time series of titles added, and forecasts the next
  18 months using Facebook's [Prophet](https://facebook.github.io/prophet/)
- Generates a short, plain-language narrative summary of the forecast
  using the Claude API
- Presents everything in an interactive Power BI dashboard: KPI cards,
  a history + forecast trend line, and genre/country breakdowns with
  filters

## Dashboard preview

![Dashboard screenshot](screenshots/dashboard_overview.png)

*(add your exported screenshot here — see "Screenshots" below)*

## How it works

| Step | Script | What it does |
|---|---|---|
| 1 | [`scripts/01_clean_and_forecast.py`](scripts/01_clean_and_forecast.py) | Loads the raw Kaggle CSV, cleans missing values, builds a monthly time series, and trains a Prophet model to forecast the next 18 months |
| 2 | [`scripts/02_ai_summary.py`](scripts/02_ai_summary.py) | Takes the forecast output and asks the Claude API to write a short narrative summary, ready to paste into the dashboard |
| 3 | — | The cleaned data + forecast CSVs are loaded into Power BI (`Netflix_forecast.pbix`) to build the dashboard itself |

## Getting started

1. Download `netflix_titles.csv` from the [Kaggle dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows)
   (not included in this repo — see `.gitignore`)
2. Run `scripts/01_clean_and_forecast.py` (Google Colab is the easiest way —
   no local setup needed) to produce:
   - `output/netflix_titles_clean.csv`
   - `output/netflix_forecast.csv`
3. (Optional) Run `scripts/02_ai_summary.py` with your own
   [Anthropic API key](https://console.anthropic.com) to generate the
   narrative summary text
4. Open `Netflix_forecast.pbix` in Power BI Desktop, and point it at the
   CSVs produced in step 2

### Requirements

```
pip install -r requirements.txt
```

## Notable challenges solved along the way

- **CSV encoding issues**: the raw Kaggle export occasionally gets mangled
  when re-saved through Excel (wrapped quoting + semicolon delimiters).
  The cleaning script auto-detects and repairs this.
- **Locale mismatches in Power BI**: forecast values imported with a `.`
  decimal separator were misread under a Dutch locale, producing absurd
  numbers — fixed by explicitly setting column locale to `en-US` during
  import.
- **Keeping history and forecast visually distinct**: rather than filtering
  the whole visual (which would have hidden real historical data), the
  forecast lines are shown only from their actual start date using
  conditional DAX measures, while the "actual" line keeps the full history.

## Tech stack

Python (pandas, Prophet, Anthropic API) · Power BI · DAX

## Data source

[Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows)
dataset on Kaggle, published by Shivam Bansal.
